# Proposed Standard for Web Pages Describing Assets Issued over the Bitcoin Network

Original proposal by Gideon Greenspan (http://www.gidgreen.com/ and gideon AT coincolors DOT org) with some input from Ron Gross (https://github.com/ripper234) and J.R. Willett (https://github.com/dacoinminster).

## Summary

This is a proposal under consideration for a standard format for web pages representing assets which are issued over the bitcoin network. Reasons for developing such a standard:

* Make it easier for wallet and tool developers to support multiple "Bitcoin 2.0" protocols (such as MasterCoin or colored coins) which might issue and transact in these assets.

* Make it easy for issuers to offer the same asset over multiple protocols.

* Increase the liquidity of such assets by maintaining their legal and financial equivalence between different protocols.

### Overview

A web page defines an asset using a similar style to [microformats](http://microformats.org). All of the fields describing the asset use regular HTML elements, which are visibile in a web browser by default. These HTML elements are given special CSS classes to indicate their additional meaning in terms of defining an asset. Since multiple CSS classes can be assigned to an element in HTML, presentation-related CSS can be freely used alongside these special classes.

A web page may include the definition for one or more types of bitcoin asset. Each asset definition is enclosed within an element with class `bitcoin-asset`. Each field in the definition uses an HTML element with a class prefixed by `bitcoin-asset-`, for example `bitcoin-asset-name`. Asset definition web pages should take care to ensure that these elements are properly closed and nested, so they won't cause fragile HTML parsers to choke.

Asset definition web pages must use UTF-8 encoding.

### Example

Below is an example of how an asset definition can be embedded in an HTML web page:

```
<div class="bitcoin-asset">
	<img class="bitcon-asset-icon-url image-center" href="http://www.john-doe-dining.com/bitcoin-credits-icon.png"/>
	<h1>Name: <span class="bitcoin-asset-name">Credit at John Doe's</span></h1>
	<h2>Description: <span class="bitcoin-asset-description">Can be used for any purchase at John Doe's, excluding breakfasts.</span></h2>
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
* `contract-url` = absolute URL of the contract underlying the asset.
* `redemption-url` = absolute URL of the web page where the asset can be redeemed.
* `work-url` = absolute URL of the content to which the asset grants a license.
* `multiple` = floating point number by which to multiply the asset quantity for display (`e` character not permitted).
* `format` = how to display asset quantities (contains `*` which is substituted for the display value).
* `format-1` = how to display the asset quantity if the display value is exactly 1.
* `color` = HTML-style hexadecimal color for displaying the asset.
* `icon-url` = absolute URL for icon to show for the asset (PNG only).
* `feed-url` = absolute URL of an RSS 2.0 feed for the asset.

Any HTML element type can be used to represent these fields. For URL fields, the URL is taken from the `href` attribute of the HTML element which has the `bitcoin-asset-` class. For other fields, the content is taken from the inside of the HTML element which has the `bitcoin-asset-` class. HTML formatting may be included inside that content, though many wallets are expected to strip this formatting and display the content as plain text.

Additional user-defined fields are permitted. Classes which are specific to a protocol should be prefixed with that protocol's name, such as `bitcoin-asset-mastercoin-id` and `bitcoin-asset-coincolors-id` above.

### Notes

* Redirections from the web page are permitted in order to provide a more user-friendly URL for bookmarking, but these should be done via Javascript or a `<meta http-equiv="refresh" ...>` rather than HTTP 301 or 302 responses. In addition the metadata should still be delivered in the original request. This prevents wallets from worrying about edge cases such as redirection loops.

* Having the contract as a separate referenced file (rather than inline in the metadata) allows it to be displayed or downloaded by any web browser as well as by wallets.

* Pages must use UTF-8 encoding to make life easy for wallet developers. In addition, all references to external assets in the metadata should use absolute URLs, to avoid wallets having to resolve relative URLs.

* If there are multiple HTML elements within a single asset definition with the same class`bitcoin-asset-...` class, then only the first element should be considered.

* Two relatively simple asset types can be defined in terms of (a) redemption (bank promises a dollar per asset unit), or (b) license (website gives access to its content for asset holders). For each of these cases, there is a field linking to the redemption process (`redemption`) or the licensed work (`work`).

* Linked contracts must be in a self-contained format such as PDF, UTF-8 encoded plain text, JPEG or PNG. The file type will be determined based on the URL suffix `.pdf`, `.txt`, `.jpg`/`.jpeg` or `.png` so that wallets don't need to read the MIME type returned by the web server. Wallets must explicitly block contracts in HTML format, since HTML web pages can reference external assets such as images whose substitution can completely change the HTML's meaning.

* The `multiple`, `format`, `format_1`, `color` and `icon` fields enable some additional control over how asset quantities are displayed in wallets. Icons should be square (with transparency permitted) and at least 32x32 pixels in size, which the wallet can scale as necessary.

* The feed field enables notifications to be issued to asset holders via RSS 2.0, for inclusion in a wallet news feed. Any critical notifications should be also posted to the asset definition web page itself.

* The attribute `style="display:none;"` can be used to make any field invisible in web browsers.