# Proposed Standard for Asset Issuance over Bitcoin

Original proposal by Gideon Greenspan (http://www.gidgreen.com/ and gideon AT coincolors DOT org) with some input from Ron Gross (https://github.com/ripper234?source=c) and J.R. Willett (https://github.com/dacoinminster).

# Summary

This is a proposal under consideration for a standard format for asset issuance over the bitcoin blockchain. The goal is to make it easier for wallet and tool developers to support multiple "Bitcoin 2.0" protocols for issuing and transacting in these assets, including MasterCoin, colored coins, etc...

The proposal has two parts:

* [Asset definition web page](#asset-definition-web-page)
* [Asset issuance transaction format](#asset-issuance-transaction-format)

From the perspective of MasterCoin, the transaction format may be of less interest, since it is based on the assumption that the issuance must take place within 36 bytes. This limit derives from the 40 bytes permitted after an OP_RETURN in bitcoin 0.9, minus 4 bytes for a issuance identifier (or in the case of MasterCoin, a transaction version and type).

# Asset definition web page

## URL

An new type of asset is issued by a single bitcoin transaction. The URL of the web page defining the asset is determined by three pieces of information from this transaction:

1. The domain name of the issuer.
2. Whether to use the prefix `http://` or `https://`.
3. The transaction ID.

The ID is given by the bitcoin protocol itself. A possible representation for items 1 and 2 in the transaction OP_RETURN data is included [below](#asset-issuance-transaction-format), but other representations may also be considered, and this will not affect the contents of the web page itself.

The full URL is the concatenation of:

1. `http://` or `https://` as appropriate.
2. The domain name encoded in the asset genesis transaction.
3. `/bitcoin-asset-`
4. The first 16 characters of the lower case hexadecimal ID of the issuance transaction.
5. `.html`

By insisting that the web page is at the root of the issuer's domain, we avoid situations where users of basic page hosting services can make their assets appear to be issued by that hosting service. Some of these services use subdirectories to host the website of each user. Placing a specific file in the root directory of a site is one of the methods that Google Webmaster Tools uses to establish site ownership, so it is reasonable to assume that it is safe.


## Web page content

The asset definition web page should provide human-readable information about the asset, along with some metadata within the `<head>` section of the page. Each metadata field in the `<head>` section takes the following form:

`<meta name="bitcoin-asset:key" content="value">`

The following fields are standard - each item begins with its key:

* `name` = name of the asset for display in wallets.
* `description` = more information about the asset (up to a few sentences).
* `contract` = absolute URL of the contract underlying the asset.
* `redemption` (optional) = absolute URL of web page where the asset can be redeemed.
* `work` (optional) = absolute URL of web page of the licensed content.
* `format` (optional) = how to display quantity (where `*` in the string is substituted for the number).
* `format_1` (optional) = how to display quantity if the display value is exactly 1.
* `color` (optional) = HTML-style hexadecimal color for displaying the asset.
* `icon` (optional) = absolute URL for icon to show for the asset (PNG only).
* `feed` (optional) = absolute URL of RSS 2.0 feed for the asset.

## Notes on asset definition web page

* In the URL, 16 characters are enough to ensure uniqueness without creating files names that are unreadable in directory listings, or too long for some operating systems.

* Redirections from the web page are permitted in order to provide a more user-friendly URL for bookmarking, but these should be done via Javascript or a `<meta http-equiv="refresh" ...>` rather than via HTTP 301 or 302 responses, and the metadata should still be delivered in the original request. This prevents wallets from worrying about edge cases such as redirection loops.

* Having the contract as a separate referenced file (rather than inline in the metadata) allows display/download in any web browser as well as wallet software.

* To make life easy for wallet developers, pages must use UTF-8 encoding. In addition, all references to external assets in the metadata should use absolute URLs, to avoid wallets having to resolve relative URLs.

* Two relatively simple asset types can be defined in terms of (a)  redemption (bank promises a dollar per asset unit), or (b) license (website promises access to its content for asset holders). Each of these has an optional metadata field linking to the redemption process or the licensed work.

* The linked contract must be in a self-contained format such as PDF, UTF-8 encoded plain text, JPEG or PNG. The file type will be determined based on the URL suffix `.pdf`, `.txt`, `.jpg`/`.jpeg`, `.png` so that wallets don't need to read the MIME type returned by the web server.)Wallets must explicitly prevent contracts being accepted if they are in HTML format, since HTML web pages can reference external assets such as images whose substitution could completely change the HTML's meaning.

* The `format`, `format_1`, `color` and `icon` fields enable some control over how the quantities should be displayed in wallets. Icons should be square (which transparency permitted) and at least 32x32 pixels in size, which the wallet can scale as necessary.

* The feed field enables notifications to be issued to color holders via RSS 2.0, for display in the appropriate fashion inside a wallet. Any critical notifications should also be posted to the human-readable asset page itself.

# Asset issuance transaction format

## Overview of fields

An asset issuance that fits within 36 bytes after an OP_RETURN contains the following fields:

1. Quantity of units created (mantissa). 2 bytes containing an unsigned 16-bit integer (small endian) between `1` and `18447`. Any other values render the issuance invalid invalid.

2. Quantity of units created (exponent) + exponent for quantity presentation. 1 byte. Most significant 4 bits: unsigned base 10 exponent for asset quantity, e.g. `0011` means multiply the mantissa by 1000 to obtain the final quantity. Least significant 4 bits: unsigned base 10 exponent for presentation plus 8, e.g. `0011` means multiply by 10<sup>-5</sup> before displaying to the user.

3. Domain name where asset web page is hosted. Up to 24 bytes, 2 bytes per 3 characters. There are only 38 permitted domain name characters. We add two end markers, one for `http://` and one for `https://`, to make a total of 40 possible characters. Since 40<sup>3</sup> < 2<sup>16</sup> we can fit 3 characters in 2 bytes, as [described below](#domain-name-representation).

4. Hash of key information from the asset definition web page. Uses the remaining bytes up to the limit of 36. Contains the prefix of `sha256(name + "\n" + description + "\n" + contract)` where the name, description and contract are retrieved via the metadata on the asset web page.

## Domain name representation

Domain names are encoded with 2 bytes per 3 characters. Each pair of adjacent bytes is interpreted as an unsigned 16-bit integer, stored in small-endian order. That integer, which we shall call `x`, will have a value between `0` and `65535`. Three ordered numbers between `0` and `39` are extracted from that integer according to the following formulae:

1. `x` modulo 40
2. `(x ÷ 40)` modulo 40
3. `(x ÷ 1600)` modulo 40

Each of these numbers between 0 and 39 is interpreted as follows:

0 ... 9 = the digits `0` to `9` in numerical order
10 ... 35 = the letters `a` to `z` in alphabetical order
36 = `-` (hyphen)
37 = `.` (period)
38 = end marker for `http://`
39 = end marker for `https://`

Domain names are decoded by repeatedly fetching pairs of bytes and interpreting them according to this scheme. As soon as an end marker is observed in the generated sequence of characters, decoding stops. No matter where the end marker appears in the set of 3 characters decoded from a 2-byte integer, both bytes are considered "burnt" in the decoding stream.

## Notes on asset issuance transaction format

* A hash of key information from the asset web page (including the full contract) must be included in the blockchain, otherwise there is no objective proof of the meaning of the asset, and asset holders won't be able to take issuers to court if they do not uphold their promise or try to change the terms of the contract.

* The combination of the asset quantity mantissa and exponent allows any quantity of an asset between 1 and 1.8447x10<sup>19</sup> to be represented with four significant digits of accuracy in just 20 bits. Note that this is sufficient to reach the highest 64-bit unsigned integer 1.84467x10<sup>19</sup>. The quantity should be interpreted subject to this upper bound.

* The presentation bits determine how asset quantities should be displayed in user wallets. For example, an asset issued to a resolution of 1 cent may still wish to have its quantities presented in dollars. In this case, the exponent for presentation would be -2.

* This scheme assumes an integer value of 1 as the smallest unit for every asset, so there is no need to distinguish between divisible and indivisible assets.

* Apart from allowing wallets to find the asset definition web page, the domain acts as a layperson's proof of the issuer's identity. One can imagine all sorts of digital signature schemes as well, but these mean a lot less to regular users than simply checking if the domain name matches a website they trust.

* The longer the domain name, the less room remains within 36 bytes for the hash prefix. However a short domain name like `hsbc.com` (8 characters) can be encoded in just 6 bytes, leaving 27 bytes for the hash, which is close to the limit of 32 bytes output by `sha256()`.