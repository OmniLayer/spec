# Proposed Standard for Bitcoin Assets

Original proposal by [Gideon Greenspan](http://www.gidgreen.com/) with input from [J.R. Willett](https://github.com/dacoinminster), [Ron Gross](https://github.com/ripper234) and [Flavien Charlon](https://github.com/Flavien).

## Summary

This is a proposal under consideration for a standard format for representing assets which are issued over the bitcoin network. Reasons for developing such a standard:

* Make it easier for wallet and tool developers to support multiple "Bitcoin 2.0" protocols (such as MasterCoin or colored coins) which might issue and transact in these assets.

* Make it easy for issuers to offer the same asset over multiple protocols.

* Increase the liquidity of such assets by maintaining their legal and financial equivalence between different protocols.

### Overview and example

Assets definitions use [JSON](http://www.json.org) format with UTF-8 encoding. Wallets must support JSONs up to 1 MB in size (they may also accept larger JSONs if they wish). Below is an example - note that all fields are optional:

```
{
	"name": "Credit at John Doe's",
	"name_short": "JD Credits",
	"issuer": "John Doe's Restaurant Chain",
	"type": "Credit",
	"currency": "USD",
	"description": "Can be used for any purchase at John Doe's, excluding breakfasts.",

	"interest_rate": 1,
	"issue_date": "2014-03-01",
	"expiry_date": "2024-02-29",

	"icon_url": "http://www.john-doe-dining.com/bitcoin-credits-icon.png",
	"image_url": "http://www.john-doe-dining.com/bitcoin-credits-image.png",
	"contract_url": "http://www.john-doe-dining.com/bitcoin-credits-contract.pdf",
	"redemption_url": "http://www.john-doe-dining.com/bitcoin-credits-redeem.php",
	"feed_url": "http://www.john-doe-dining.com/bitcoin-credits-feed.rss",
	
	"color": "#ffccaa",
	"multiple": 0.01,
	"format": "* dollars",
	"format_1": "1 dollar",
	
	"meta_id": "123456",
	"meta_source": "mastercoin|coloredcoin|futureproof",
	"source_data": ["2MyvmsxoCxsnzPapDnK7ZcMxWjU3hqLZkpX"]
}
```

### List of fields

All fields are optional in this specification, though some may be required by certain protocols:

* `name` = full name of the asset for display, up to 64 characters.
* `name_short` = short name of the asset for display, up to 16 characters.
* `issuer` = full legal name of the issuer for display.
* `type` = type of the asset, examples: `"Credit"`, `"Stock"`, `"Bond"`, `"Cash"`, `"Licence"`, `"Coupon"`, `"Property"`.
* `currency` = 3-character [ISO 4217](http://en.wikipedia.org/wiki/ISO_4217) currency code, if relevant.
* `description` = more information about the asset for display (up to a few sentences).
* `interest_rate` = annual interest rate percentage, negative for demurrage.
* `issue_date` = date/time when the asset was issued, formatted as [ISO 8601](http://en.wikipedia.org/wiki/ISO_8601).
* `expiry_date` = date/time when the asset will no longer be valid, formatted as ISO 8601.
* `icon_url` = absolute URL for square icon to show for the asset (PNG only), minimum 32x32 pjxels. [1 MB]
* `image_url` = absolute URL for larger image to show for the asset (PNG or JPEG), minimum 128x128 pjxels. [1 MB]
* `contract_url` = absolute URL of the contract underlying the asset. [16 MB]
* `redemption_url` = absolute URL of the web page where the asset can be redeemed.
* `work_url` = absolute URL of the content to which the asset grants a license.
* `feed_url` = absolute URL of an RSS 2.0 feed for the asset. [1 MB]
* `color` = HTML-style hexadecimal color for displaying the asset, with preceding `#`.
* `multiple` = number by which to multiply the asset quantity for display.
* `format` = how to display asset quantities (`*` is substituted for the display value), up to 20 characters.
* `format_1` = how to display the asset quantity if the display value is exactly 1, up to 20 characters.

For `*_url` fields, sizes in [square brackets] indicate the size of the referenced asset that wallets must support. Wallets may support larger sizes if they wish.

Additional user-defined fields are permitted. Fields which are specific to a protocol should be prefixed with that protocol's name, such as `mastercoin_id`, `coinprism_sources` and `coincolors_id` in the example above.

### Notes

* All references to external assets in the metadata should use absolute URLs, to avoid wallets having to resolve relative URLs.

* Two relatively simple asset types can be defined in terms of (a) redemption (e.g. a bank promises one cent per asset unit), or (b) license (a website grants access to its content for asset holders). For these cases, there is a field linking to the redemption process (`redemption_url`) or the licensed work (`work_url`) respectively.

* Linked contracts (`contract_url`) must be in a self-contained format such as PDF (without external references), UTF-8 encoded plain text, JPEG or PNG. The file type will be determined based on the MIME type returned by the web server in the HTTP headers. If the MIME type was incorrect, wallets may optionally use file inspection to determine the file's contents. Wallets must explicitly block contracts in HTML format, since HTML web pages can reference external assets such as images whose substitution can completely change their meaning.

* The `format`, `format_1`, `color`, `icon_url` and `image_url` fields enable visual control over how asset quantities are displayed in wallets. Icons should be square PNGs (with transparency permitted) at least 32x32 pixels in size, which the wallet can scale as necessary. Images can be PNGs or JPEGs and should be at least 128x128 pixels in size, not necessarily square.

* If present, the `multiple` and `interest_rate` fields modify the raw integer quantity of units, before it is displayed in a wallet. (The raw number of units held can only be changed by transactions on the blockchain.) The `interest_rate` field is treated as a percentage per annum, beginning from the `issue_date`, and may be negative to indicate demurrage. In quasi-code, the display amount would be calculated as:

```
display=raw_units_held;

multiple=get_asset_definition_field('multiple');
interest_rate=get_asset_definition_field('interest_rate');
issue_date=get_asset_definition_field('issue_date');
format=get_asset_definition_field('format');
format_1=get_asset_definition_field('format_1');

if (is_valid_floating_point(interest_rate) && is_valid_iso_8601(issue_date)) {
	seconds_elapsed=time_now_in_seconds()-iso_8601_to_seconds(issue_date);
	years_elapsed=seconds_elapsed/31557600 // assume 365.25 days per year;
	display*=power(1.0+interest_rate/100, years_elapsed);
}

if (is_valid_floating_point(multiple))
	display*=multiple;
	
if ((display==1.0) && is_not_empty(format_1))
	display=format_1;
	
else if (is_not_empty(format) && string_contains(format, '*'))
	display=string_replace(format, '*', display);

```

* The `expiry_date` field indicates when the asset will no longer be redeemable, or no longer grant access to the licensed content. Wallets should display an appropriate warning as this expiry date approaches, in order to remind users to redeem their asset or renew their subscription.

* The `feed_url` field enables notifications to be issued to asset holders via RSS 2.0, for inclusion in a wallet news feed.

* An asset definition JSON can be embedded inside a web page by `\uXXXX`-escaping the characters `(` `)` `<` `>` inside the JSON, and inserting it into the code below. Using this encoding, the JSON can be easily extracted from the raw HTML using a regular expression (no DOM parsing) and will also be accessible to Javascript on the page.

```
<script>
if (typeof _bitcoin_asset_specification_ === "function")
	_bitcoin_asset_specification_(JSON_HERE);
</script>
```
