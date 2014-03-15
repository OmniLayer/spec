# Proposed Standard for Web Pages Describing Assets Issued over the Bitcoin Network

Original proposal by [Gideon Greenspan](http://www.gidgreen.com/) with input from [J.R. Willett](https://github.com/dacoinminster) and [Ron Gross](https://github.com/ripper234).

## Summary

This is a proposal under consideration for a standard format for web pages representing assets which are issued over the bitcoin network. Reasons for developing such a standard:

* Make it easier for wallet and tool developers to support multiple "Bitcoin 2.0" protocols (such as MasterCoin or colored coins) which might issue and transact in these assets.

* Make it easy for issuers to offer the same asset over multiple protocols.

* Increase the liquidity of such assets by maintaining their legal and financial equivalence between different protocols.

### Overview

A web page defines an asset using a similar style to [microformats](http://microformats.org). All of the fields describing the asset use regular HTML elements, which are visibile in a web browser by default. These HTML elements are given special CSS classes to indicate their additional meaning in terms of defining an asset. Since multiple CSS classes can be assigned to an element in HTML, presentation-related CSS can be freely used alongside these special classes.

A web page may include the definition for one or more types of bitcoin asset. Each asset definition is enclosed within an element with class `bitcoin-asset`. Each field in the definition uses an HTML element with a class prefixed by `bitcoin-asset-`, for example `bitcoin-asset-name`. Asset definition web pages should take care to ensure that these elements are properly closed and nested, so they won't cause fragile HTML parsers to choke.

Asset definition web pages must use UTF-8 encoding and their source HTML (excluding externally referenced assets) must be no larger than 1 MB in size.

### Example

Below is an example of how an asset definition can be embedded in an HTML web page:

```
<div class="bitcoin-asset">
	<img class="bitcon-asset-icon-url image-center" href="http://www.john-doe-dining.com/bitcoin-credits-icon.png"/>
	<h1>Name: <span class="bitcoin-asset-name">Credit at John Doe's</span></h1>
	<h2>Description: <span class="bitcoin-asset-description">Can be used for any purchase at John Doe's, excluding breakfasts.</span></h2>
	<p>Interest rate: <span class="bitcoin-asset-interest">1</span>% per annum, from <span class="bitcoin-asset-issue-date">2014-03-01</span>.</p>
	<p>Expiry date: <span class="bitcoin-asset-expiry-date">2024-02-29</span>. After this date, these credits cannot be redeemed.</p>
	<p><a class="bitcoin-asset-contract-url" href="http://www.john-doe-dining.com/bitcoin-credits-contract.pdf">View contract</a></p>
	<p><a class="bitcoin-asset-redemption-url" href="http://www.john-doe-dining.com/bitcoin-credits-redeem.php">Use your credits now</a></p>
	<p><a class="bitcoin-asset-feed-url" href="http://www.john-doe-dining.com/bitcoin-credits-feed.rss">News from John Doe's</a></p>
	<span class="bitcoin-asset-color" style="display:none;">#ffccaa</span>
	<span class="bitcoin-asset-multiple" style="display:none;">0.01</span>
	<span class="bitcoin-asset-format" style="display:none;">* dollars</span>
	<span class="bitcoin-asset-format-1" style="display:none;">1 dollar</span>
	<span class="bitcoin-asset-mastercoin-id" style="display:none;">123456</span>
	<span class="bitcoin-asset-coincolors-id" style="display:none;">f5d8ee39a430901c91a5917b9f2dc19d6d1a0e9cea205b009ca73dd04470b9a6</span>
</div>
```

### List of fields

All fields are optional in this specification, though some may be required by certain protocols. All field names should be prefix by `bitcoin-asset-`:

* `name` = name of the asset for display in wallets.
* `description` = more information about the asset (up to a few sentences).
* `contract-url` = absolute URL of the contract underlying the asset, maximum size 16 MB.
* `redemption-url` = absolute URL of the web page where the asset can be redeemed.
* `work-url` = absolute URL of the content to which the asset grants a license.
* `issue-date` - date/time when the asset was issued, formatted as [ISO 8601](http://en.wikipedia.org/wiki/ISO_8601).
* `expiry-date` = date/time when the asset will no longer be valid, formatted as ISO 8601.
* `multiple` = floating point number to multiply the asset quantity for display (`e` character not permitted).
* `interest` = floating point percentage interest rate, negative for demurrage (`e` character not permitted).
* `format` = how to display asset quantities (`*` is substituted for the display value).
* `format-1` = how to display the asset quantity if the display value is exactly 1.
* `color` = HTML-style hexadecimal color for displaying the asset.
* `icon-url` = absolute URL for icon to show for the asset (PNG only), maximum size 1 MB.
* `feed-url` = absolute URL of an RSS 2.0 feed for the asset, maximum size 1 MB.

Any HTML element type can be used to represent these fields. For `-url` fields, the URL is taken from the `href` attribute of the HTML element which has the `bitcoin-asset-...-url` class. For other fields, the content is taken from the inside of the HTML element which has the `bitcoin-asset-...` class. For the `name`, `description`, `format` and `format-1` fields, HTML formatting may be included inside that content, though many wallets are expected to strip this formatting and display the content as plain text.

Additional user-defined fields are permitted. Classes which are specific to a protocol should be prefixed with that protocol's name, such as `bitcoin-asset-mastercoin-id` and `bitcoin-asset-coincolors-id` above.

### Notes

* Redirections from the web page are permitted in order to provide a more user-friendly URL. It is preferable for redirectiona to take place via Javascript or a `<meta http-equiv="refresh" ...>` tag rather than HTTP 301 or 302 responses, with the metadata still being delivered in the original request. This will help wallets to load the asset definition quickly. In any event, a maximum of 5 redirection hops are permitted.

* Having the contract as a separate referenced file (rather than inline in the metadata) allows it to be displayed or downloaded by any web browser as well as by wallets.

* Pages must use UTF-8 encoding to make life easy for wallet developers.

* All references to external assets in the metadata should use absolute URLs, to avoid wallets having to resolve relative URLs.

* If there are multiple HTML elements within a single asset definition with the same `bitcoin-asset-...` class, then only the first element should be considered.

* Two relatively simple asset types can be defined in terms of (a) redemption (e.g. a bank promises one cent per asset unit), or (b) license (a website grants access to its content for asset holders). For these cases, there is a field linking to the redemption process (`redemption-url`) or the licensed work (`work-url`) respectively.

* Linked contracts must be in a self-contained format such as PDF, UTF-8 encoded plain text, JPEG or PNG. The file type will be determined based on the URL suffix `.pdf`, `.txt`, `.jpg`/`.jpeg` or `.png` so that wallets don't need to interpret the MIME type returned by the web server. Wallets must explicitly block contracts in HTML format, since HTML web pages can reference external assets such as images whose substitution can completely change their meaning.

* If present, the `multiple` and `interest` fields modify the raw integer quantity of units, before it is displayed in a wallet. (The raw number of units held can only be changed by transactions on the blockchain.) The `interest` field is treated as a percentage per annum, beginning from the `issue-date`, and may be negative to indicate demurrage. In quasi-code, the display amount would be calculated as:

```
display=raw_units_held;
interest=get_asset_definition_field('interest');
issue_date=get_asset_definition_field('issue-date');

if (is_valid_floating_point(interest) && is_valid_iso_8601(issue_date)) {
	seconds_elapsed=time_now_in_seconds()-iso_8601_to_seconds(issue_date);
	years_elapsed=seconds_elapsed/31557600 // assume 365.25 days per year;
	display*=power(1.0+interest/100, years_elapsed);
}

if (is_valid_floating_point(multiple))
	display*=multiple;
```

* The `expiry-date` field indicates when the asset will no longer be redeemable, or no longer grant access to the licensed content. Wallets should display an appropriate warning as this expiry date approaches, in order to remind users to redeem their asset or renew their subscription.

* The `format`, `format_1`, `color` and `icon-url` fields enable some additional control over how asset quantities are displayed in wallets. Icons should be square (with transparency permitted) and at least 32x32 pixels in size, which the wallet can scale as necessary.

* The `feed-url` field enables notifications to be issued to asset holders via RSS 2.0, for inclusion in a wallet news feed. Any critical notifications should be also posted to the asset definition web page itself.

* The attribute `style="display:none;"` can be used to make any field invisible in web browsers.

* Online wallets should always display asset definition web pages in separate a frame, iframe, tab or window, in order to prevent cross-site scripting (XSS) attacks by Javascript on the page.