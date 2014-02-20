The Master Protocol / Mastercoin Complete Specification
=======================================================

Version 0.4 Transaction versions & 5 initial transactions Edition

* JR Willett (https://github.com/dacoinminster and jr DOT willett AT gmail DOT com)
* Maran Hidskes (https://github.com/maran)
* David Johnston (https://github.com/DavidJohnstonCEO)
* Ron Gross (https://github.com/ripper234?source=c)
* Marv Schneider (https://github.com/marv-engine)

With input by Peter Todd (https://github.com/petertodd)

# Summary

We claim that the existing bitcoin network can be used as a protocol layer, on top of which new currency layers with new rules can be built without changing the foundation.  We further claim that the new protocol layers described in this document:

* Will fix the two biggest barriers to widespread bitcoin adoption: instability and insecurity.
* Will financially benefit the entire bitcoin user community, including those who don’t use the new protocol layers.
* Will provide initial funds to hire developers to build software which implements the new protocol layers, and ongoing funds to pay for maintenance of this software.
* Will richly reward early adopters of the new protocol, in proportion to how successful it is.


# Assumptions

Our claims are built on the following assumptions:

* Alternate block chains compete with bitcoins financially, confuse our message to the world, and dilute our efforts. These barriers interfere with the adoption momentum of bitcoin and the adoption momentum of alternate currencies as well, regardless of how well-conceived their rules may be.
* New protocol layers on top of the bitcoin protocol will increase bitcoin values, consolidate our message to the world, and concentrate our efforts, while still allowing individuals and groups to issue new currencies with experimental new rules. The success of any experimental currency protocol layer will enhance the value and success of the foundational bitcoin protocol. 
* Getting consensus and widespread adoption from the bitcoin community is not needed to add protocol layers, since no changes to the foundational bitcoin protocol are required. 
* Tiny bitcoin transactions can be encoded into the block chain to support and represent transactions in higher protocol layers. 
* A protocol can pay for its own software development, “bootstrapping” itself into existence, utilizing a trusted entity to hold funds and hire developers. 
* It is possible to create tools to allow end users to create currency protocol layers which have a stable value, pegged to an external currency or commodity. In this way, users of these currencies can own stabilized virtual currency tied to U.S. Dollars, Euros, ounces of gold, barrels of oil, etc. 
* It is possible for users of these new currencies to exchange between currencies with each other using simple rules and no central exchange.


# Visualization

The proposed protocol layers can be visualized as follows, with arrows representing users exchanging between currencies:

![Mastercoin Protocol Layers](images/layers.png) 


Note that all transfers of value are still stored in the normal bitcoin block chain, but higher layers of the protocol assign additional meaning to some transactions.

# Document History

1. Version 0.1 (previously 0.5) released 6 Jan 2012 (No packet definitions, overly-complicated currency stabilization)
2. Version 0.1.9 (previously 0.7) released 29 Jul 2013 (Preview of 0.2, but without revealing the Exodus Address)
3. Version 0.2 (previously 1.0) released 31 Jul 2013 (Version used during the fund-raiser)
4. Version 0.3 (previously 1.1) released 9 Sep 2013 (Smart Property + improvements for easier parsing & better escrow fund health)
5. Version 0.3.5 (previously 1.2) released 11 Nov 2013 (Added "Pay Dividend" message, spending limits for savings wallets, contract-for-difference bets, and distributed e-commerce messages. Also added Zathras' new appendix (description of class B and class C methods of storing Mastercoin data).
6. Version 0.4 released 15 Feb 2014 (defined transaction message fields in a separate section, specified 5 transactions for initial deployment, added transaction version, New/Update/Cancel for sell offers, corrected dust threshold value) 

* Pre-github versions of this document (prior to version 0.3.5 / previously 1.2) can be found at https://sites.google.com/site/2ndbtcwpaper/

# Master Protocol / Mastercoin Terminology

* The term M.A.S.T.E.R. is an acronym for "Metadata Archival of Standard Transaction Embedding Records" 
* The term "Master Protocol" applies to the specification and the clients that implement its features.
* The term "MSC Protocol" is used as the abbreviation for "Master Protocol". 
* The term "Mastercoins" applies to the digital tokens that access the features of the "Master Protocol" clients.
* The term "MSC" is used as the abbreviation for "Mastercoins".

# Master Protocol Design

The “Master Protocol” layer between the existing Bitcoin Protocol and users’ currencies is intended to be a base upon which anyone can build their own currency. The software implementing the Master Protocol will contain simple tools which will allow anyone to design and release their own currency with their own rules without doing any software development.

## Initial Token Distribution via the “Exodus Address”

Perhaps you have heard of the “Genesis Block” which launched the Bitcoin protocol. The Master Protocol has a similar starting point in the block chain, called the “Exodus Address” - the bitcoin address from which the first Mastercoins were generated during the month of August 2013. The Exodus Address is: **[1EXoDusjGwvnjZUyKkxZ4UHEf77z6A5S4P](https://blockchain.info/address/1EXoDusjGwvnjZUyKkxZ4UHEf77z6A5S4P)**  

Initial distribution of Mastercoins was essentially a kickstarter style period to provide funding to pay developers to write the software which fully implements the protocol. The distribution was very simple, and proceeded as follows:

1. Anyone who sent bitcoins to the Exodus Address before August 31st, 2013 was recognized by the protocol as owning 100x that number of Mastercoins. For instance, if I sent 100 bitcoins to the Exodus Address before August 31st, my bitcoin address owns 10,000 Mastercoins after August 31st. 
2. Early buyers got additional Mastercoins. In order to encourage adoption momentum, buyers got an additional 10% bonus Mastercoins if they made their purchase a week before the deadline, 20% extra if they purchased two weeks early, and so on, including partial weeks. Thus, if I sent 100 bitcoins to the exodus address 1.5 weeks before August 31st, the protocol recognized my bitcoin address as owning 11,500 Mastercoins (10000 + 15% bonus).
3. Attempts to send funds to the Exodus Address on or after September 1st 2013 (after block #255365) were not considered Mastercoin purchases, and were refunded to the sender.

In the event that a purchase had multiple inputs, the input address contributing the most funds was recognized as owning the Mastercoins.

Note that anyone who purchased Mastercoins also received the same number of “Test Mastercoins” which are being used for testing new features before they are available for use in the Mastercoin protocol.

Initially, the only valid Mastercoin transaction was a “simple send” (defined later in this document), but the additional features described in this document are being implemented, and can be used once they are fully tested.

## Development Mastercoins (Dev MSC), previously "Reward Mastercoins")

1. Generation Rate: For every 10 Mastercoins sold during the Exodus period, 1 additional “Dev MSC” was also generated, which are being awarded to the Exodus Address slowly over the years following the exodus period (these Dev MSC are interoperable and fungible with regular MSC). These Development Mastercoins will ensure that developers have a continuing incentive to maintain, improve and add features to the Master Protocol implementations desired by users. The Distribution of these Dev MSC is structured so that developers receive 50% of the Dev MSC by one year after the initial Exodus Address period closed (date the Exodus Address closed - August 31st 2013, although transactions up till block 255365 were still accepted to account for slower propegation of transactions still sent on the 31st of August), 75% by a year later, 87.5% by a year later, and so on:

![Dev MSC](images/reward-mastercoin-formula.png)

2. Standard Distribution Rate Algorithm: The distribution of "Dev MSC", mirrors the amount of Dev MSC generated each 30.41 days. Further more these Dev MSC are distributed in proportion to the amount of BTC won by participants in Master Protocol development bounties completed during that 30.41 day period. The intension being, that the Mastercoin Foundation will handle this distribution process temporarily until its operation can be transferred to the Master Protocol community via the Distributed Bounty system and the Proof of Stake consensus mechanism. During the temporary period of the Mastercoin Foundation distributing the Dev MSC 50% of the normal distribution rate shall apply. When the Master Protocol community begins handling the Distribution of the Dev MSC all undistributed Dev MSC from the pervious period shall be transferred to the Decentralized system and consensus must be reached on the standard distribution rate algorithm for the new period going forward.

Simple Dev MSC Distribution Equation:  A / B = C * D = E
(A) Amount of awards an individual earns in BTC during a 30.41 day period, divided by,
(B) the total amount of BTC awarded during that 30.41 day period, equals
(C) his or her individual award percentage, times
(D) the total Dev MSC generated during that 30.41 day period, equals
(E) the amount of Dev MSC awarded to the individual in addition to his BTC awards during the 30.41 day period.

Example #1 (using round numbers): 
A (100 BTC) / B (1,000 BTC) = C (10%) * D (1,000 MSC) = E (100 MSC)

There are 56,316 Dev MSC that will ever be generated. 
28,158 Dev MSC will be generated the first year or 2,346 MSC each 30.41 day period. 
So if a developer won 10% of the bounties during this 30.41 day period he or she would be distributed 234.6 Dev MSC.


Technical notes: 

* Any Mastercoin implementation implementing Exodus balance should recalculate the Development Mastercoin amount on each new block found and use the block timestamp for y.
* When calculating the years since the Mastercoin sale we assume a year is 31556926 seconds.
* 1377993874 is the Unix timestamp used to define the end-date of Exodus and thus the start date for the Development Mastercoins vesting.


## Embedding Master Protocol Data in the Block Chain

Bitcoin has some little-known advanced features (such as scripting) which many people imagine will enable it to perform fancy new tricks someday. The Master Protocol uses exactly NONE of those advanced features, because support for them is not guaranteed in the future, and the Master Protocol doesn't need them to embed data in the block chain.

The Master Protocol was originally specified to embed data in the block chain using fake bitcoin addresses (Class A), but we've since come up with a more blockchain friendly method which embeds data in a bitcoin multi-signature transaction (Class B). Once bitcoin miners start supporting the new OP_RETURN opcode as part of version 0.9 of the Bitcoin reference client, Master Protocol will be able to use that opcode to make the Master Protocol data completely prune-able (Class C) see description here by Gavin Andresen here: https://bitcoinfoundation.org/blog/?p=290 

Class C transactions are most preferred due to the Provably Prune-able Outputs avoiding issues of "bloat" and "pollution" of the block chain.

The technical details for both Class A and Class B transactions can be found in Appendix A.

## Special Considerations to Avoid Invalid Transactions

Not every wallet lets you choose which address bitcoins come from when you make a payment, and Mastercoin transactions must all come from the address which holds the Mastercoins. If a wallet contains bitcoins stored in multiple addresses, the wallet must first consolidate the bitcoins by sending ALL of them to the address which is going to initiate a Mastercoin transaction. Then, any Mastercoin-related bitcoin transactions will be sent from that address.

Wallets which do not allow you to consolidate to one address and send from that address (such as online web wallet providers) will not work for Mastercoin unless they are modified to do so. For this reason, **attempting to purchase Mastercoins from an online web wallet will likely result in the permanent loss of those Mastercoins.**

## Unlocking features

Not all features described in this document are active by default. Each feature will be unlocked on a certain block once it's deemed stable. Only Test Mastercoin transactions will be allowed if a feature is not unlocked yet. All other messages will be invalidated. The only exception to this rule is the Simple Send message, this has been enabled since Exodus.

## Transaction Field Definitions

This section defines the fields that are used to construct transaction messages.

### Field: Currency identifier
+ Description: the currency used in the transaction
+ Size: two 32-bit unsigned integers (4 bytes each) or 1 such integer in special cases
+ Valid values: these form a reference to the transaction in which the currency was created. The first integer is the number of the block in the blockchain and the second is the byte offset within the block of that transaction. Since no currencies were created before block 131072, any block numbers below 131072 can be considered as special values, e.g. "1" for MasterCoin. For these special currencies, the byte offset can be skipped.

### Field: Integer-eight byte
+ Description: used as a multiplier or in other calculations
+ Size: 64-bit unsigned integer, 8 bytes
+ Valid values: 0 to 9,223,372,036,854,775,807

### Field: Integer-four byte
+ Description: used as a multiplier or in other calculations
+ Size: 32-bit unsigned integer, 4 bytes
+ Valid values: 0 to 4,294,967,295

### Field: Integer-two byte
+ Description: used as a multiplier or in other calculations
+ Size: 16-bit unsigned integer, 2 bytes
+ Valid values: 0 to 65535

### Field: Listing identifier (future)
+ Description: the unique identifier assigned to each sale listing an a per address basis
+ Size: 32-bit unsigned integer, 4 bytes
+ Valid values: 0 to 4,294,967,295

### Field: Number of coins
+ Description: Specifies the number of coins affected by the transaction this field appears in. Note: the number of coins is to be multiplied by 100,000,000 in this field (e.g. 100,000,000 represents 1.0 MSC), which allows for the number of Master Protocol-derived coins to be specified with the same precision as bitcoins (eight decimal places).
+ Size: 64-bit unsigned integer, 8 bytes
+ Valid values: 1 to 9,223,372,036,854,775,807

### Field: Property type
+ Description: indivisible or not
+ Size: 16-bit unsigned integer, 2 bytes
+ Valid values:
    * 1: Indivisible shares
    * 2: Divisible currency

### Field: Response sub-action (future)
+ Description: the seller's response to a buyer's offer to purchase
+ Size: 8-bit unsigned integer, 1 byte
+ Valid values:
    * 1: Accept
    * 2: Reject
    * 3: Contact

### Field: String null-terminated
+ Description: a variable length string terminated with a \0 byte
+ Size: variable
+ Valid values: UTF-8

### Field: Time period in blocks
+ Description: number of blocks during which an action can be performed
+ Size: 8-bit unsigned integer, 1 byte
+ Valid values: 1 to 255 

### Field: Time period in seconds (future)
+ Description: number of seconds during which an action can be performed
+ Size: 32-bit unsigned integer, 4 bytes
+ Valid values: 1 to 31,536,000 (365.0 days)

### Field: Sell offer sub-action
+ Description: the specific action to be applied to the sell offer by this transaction
+ Size: 8-bit unsigned integer, 1 byte
+ Valid values:
    * 1: New
    * 2: Update
    * 3: Cancel

### Field: Transaction type
+ Description: the MSC Protocol function to be performed
+ Size: 16-bit unsigned integer, 2 bytes
+ Inter-dependencies: [Transaction version](#field-transaction-version)
+ Current Valid values:
    *    0: [Simple Send](#transfer-coins-simple-send)
    *   20: [Sell Coins for Bitcoins (currency trade offer)](#sell-coins-for-bitcoins)
    *   21: [Offer/Accept Master Protocol-Derived Coins for Another Master Protocol-derived Currency (currency trade offer)](#sell-master-protocol-derived-coins-for-another-master-protocol-derived-currency)
    *   22: [Purchase Coins with Bitcoins (accept currency trade offer)](#purchase-coins-with-bitcoins)
    *   50: [Create a Property](#smart-property)

+ To be added in future releases:
    *    1: [Pay Dividends (Send All)](#pay-dividends-send-all)
    *   10: [Mark an Address as Savings](#marking-an-address-as-savings)
    *   11: [Mark a Savings Address as Compromised](#marking-a-savings-address-as-compromised)
    *   12: [Mark an Address as Rate-Limited](#marking-an-address-as-rate-limited)
    *   14: [Remove a Rate Limitation](#removing-a-rate-limitation)
    *   30: [Register a Data Stream](#registering-a-data-stream)
    *   40: [Offer/Accept a Bet](#offering-a-bet)
    *   60: [List Something for Sale](#listing-something-for-sale)
    *   61: [Initiate a Purchase from a Listing](#initiating-a-purchase)
    *   62: [Respond to a Buyer Offer](#accepting-a-buyer)
    *   63: [Release Funds and Leave Feedback](#leaving-feedback)
    * 100: [Create a New Child Currency](#new-currency-creation)

### Field: Transaction version
+ Description: the version of the transaction definition, monotonically increasing independently for each transaction type
+ Size: 16-bit unsigned integer, 2 bytes
+ Required/optional: Required
+ Inter-dependencies: [Transaction type](#field-transaction-type)
+ Valid values: 0 to 65535

## Transaction Definitions
The Master Protocol Distributed Exchange transactions are listed below. Transactions 0, 20, 21, 22 and 50 are to be implemented in the first deployment, per this spec. They are listed first. The other transactions will be fully defined and implemented in future releases.

Each transaction definition has its own version number to enable support for changes to each transaction definition. Up thru version 0.3.5 of this spec, the transaction type field was a 4 byte integer. Since there were only 17 transactions identified, the upper 3 bytes of the field had a value of 0. For all spec versions starting with 0.4, the first field in each transaction message is the 2 byte version number, with an initial value of 0 and the transaction type field is a 2 byte integer. So, each client must examine the first two bytes of the transaction message to determine how to parse the remainder of the message. If the value is 0, then the message is in the format specified in version 0.3.5 of this spec. If the value is at least 1, then the message is in the format associated with that version number.

Note: Master Protocol transactions are not reversible except as explicitly indicated by this spec.


###Transfer Coins (Simple Send)

Description: Transaction type 0 transfers coins in the specified currency from the sending address to the reference address, defined in [Appendix A](#appendix-a-storing-mastercoin-data-in-the-blockchain). This transaction can not be used to transfer bitcoins.

If the amount to transfer exceeds the number owned by the sending address, this indicates the user is transferring all of them.

[Future: Note that if the transfer comes from an address which has been marked as “Savings”, there is a time window in which the transfer can be undone.]

Say you want to transfer 1 Mastercoin to another address. Only 16 bytes are needed. The data stored is:

1. [Transaction version](#field-transaction-version) = 0
1. [Transaction type](#field-transaction-type) = 0
1. [Currency identifier](#field-currency-identifier) = 1 for MasterCoin 
1. [Amount to transfer](#field-number-of-coins) = 100,000,000 (1.00000000 Mastercoins)

### Sell Mastercoins for Bitcoins

Description: Transaction type 20 posts the terms of an offer to sell Mastercoins or Test Mastercoins for bitcoins. A new sell offer is created with Action = 1 (New). Valid currency identifier values for this transaction are 1 for MSC or 2 for Test MSC.

If the amount offered exceeds the number owned by the sending address, this indicates the user is offering to sell all of them. That amount will be reserved from the available balance for this address much like any other exchange platform. For instance: If an address owns 100 MSC and it creates a "Sell Order" for 100 MSC, then the address's available balance is now 0 MSC, reserving 100 MSC. Other outgoing Mastercoin transactions created while this order is still valid will be invalidated.

An address cannot create a new Sell Mastercoins for Bitcoins offer while that address has an active Sell Mastercoins for Bitcoins offer (one that has not been canceled or fully accepted and full payment received).

Say you want to publish an offer to sell 1.5 Mastercoins for 1000 bitcoins. Doing this takes 34 bytes:

1. [Transaction version](#field-transaction-version) = 1
1. [Transaction type](#field-transaction-type) = 20 (currency trade offer for bitcoins)
1. [Currency identifier](#field-currency-identifier) = 1 for Mastercoin 
1. [Amount for sale](#field-number-of-coins) = 150,000,000 (1.50000000 Mastercoins)
1. [Amount of bitcoins desired](#field-number-of-coins) = 100,000,000,000 (1000.00000000 bitcoins)
1. [Time limit in blocks](#field-time-limit-in-blocks) = 10 (10 blocks in which to send payment after counter-party accepts these terms)
1. [Minimum bitcoin transaction fee](#field-number-of-coins) = 10,000,000 (require the buyer to pay a hefty 0.1 BTC transaction fee to the miner, discouraging fake offers)
1. [Action](#field-sell-offer-sub-action) = 1 (New offer)

#### Change a Coin Sell Offer

An offer to sell coins can be changed by using Action = 2 (Update) until either: there are valid corresponding purchase offers (transaction type 22) for the whole amount offered, or the sell offer is canceled.

The change will apply to the balance that has not yet been accepted with a purchase offer. It's your responsibility to determine if the update was successful and how many coins were purchased before the update took effect.

The amount reserved from the available balance for this address will be adjusted to reflect the new amount for sale.

Say you decide you want to change an offer, e.g. the number of coins you are offering for sale, or change the asking price. Send the transaction with the new values and the values that are not changing and Action = 2 (Update) before the whole amount offered has been accepted. 

#### Cancel a Coin Sell Offer

A currency sell offer can be canceled by using Action = 3 (Cancel) until the offer has been fully accepted by valid purchase offers (transaction type 22). When a sell offer is canceled, the associated coins are no longer reserved.

The cancel will apply to the amount that has not yet been accepted. It's your responsibility to determine if the cancellation was successful and how many coins were not sold.

If you want to cancel an offer, use Action = 3 (Cancel) and send the transaction before the full amount for sale has been accepted.

### Purchase Mastercoins with Bitcoins

Description: Transaction type 22 posts acceptance of an offer to sell Mastercoins for bitcoins. All or some of the coins offered can be purchased with this transaction.

The reference address must point to the seller's address, to identify whose offer you are accepting. The purchaser’s address must be different than the seller’s address.

If you send an offer for more coins than are available by the time your transaction gets added to a block, your amount bought will be automatically adjusted to the amount still available. When a Purchase Offer is sent to an address that does not have a matching active Sell Offer, e.g. the Sell offer has been canceled or is all sold out, the Purchase Offer must be invalidated. 

Note: Your total expenditures on bitcoin transaction fees while accepting the purchase must meet the minimum fee specified in the Sell Offer in order for the transaction to be valid.

You must send the appropriate amount of bitcoins before the time limit expires to complete the purchase. Note that you must send the bitcoins from the same address which initiated the purchase. If you send less than the correct amount of bitcoins, your purchase will be adjusted downwards. The remaining coins will be added back to those available in the Sell Offer if it’s still active. If you send more than the correct amount of bitcoins and the Sell Offer has more coins still available your order will be adjusted upwards. If you do not send any payment before the time limit expires, the coins you were going to purchase will be added back to those available in the Sell Offer if it’s still active.

Please note that all transactions between the Purchase Offer and expiration block should be accumulated and that this value must be used to adjust the Purchase Offer accordingly.

In order to make parsing Master Protocol transactions easier, you must also include an output to the Exodus Address when sending the bitcoins to complete a purchase of Mastercoins. The output can be for any amount, but should be above the dust threshold.

Master Protocol messages that also have a reference output to the seller address, for instance if the buyer wants to change his offer, should not be counted towards the actual purchase of Master Protocol-derived coins. 

Say you see an offer such as the one listed above, and wish to initiate a purchase of those coins. Doing so takes 16 bytes:

1. [Transaction version](#field-transaction-version) = 0
1. [Transaction type](#field-transaction-type) = 22 (accept currency trade offer)
1. [Currency identifier](#field-currency-identifier) = 1 for Mastercoin 
1. [Amount to be purchased](#field-number-of-coins) = 130,000,000 (1.30000000 Mastercoins)

### Sell Master Protocol-Derived Coins for Another Master Protocol-Derived Currency

Description: Transaction type 21 is used to both publish and accept an offer to sell coins in a Master Protocol-Derived Currency for coins in another Master Protocol-Derived Currency.

If the amount offered exceeds the number available to be sold by the sending address, this indicates the user is offering to sell all of them. That amount will be reserved from the available balance for this address much like any other exchange platform.

An address cannot create a new Sell Master Protocol-Derived Coins for Other Master Protocol-Derived Currencies offer while that address has an active sell offer with those currencies in the same roles. An active sell offer is one that has not been canceled or fully accepted.

To accept a sell offer, simply publish the same message type with an inverse offer (e.g. selling Goldcoins for Mastercoins in the example below) at a price which matches or beats the seller's price. The protocol simply finds orders that match and the coins from matching orders are considered transferred at the price specified by the earlier of the two offers. The purchaser’s address must be different than the seller’s address.

Note that when only some coins are purchased, the rest are still for sale with the same terms.

Say you want to publish an offer to sell 2.5 Mastercoins for 50 GoldCoins (coins which each represent one ounce of gold, derived from Mastercoins and described later in this document). For the sake of example, we'll assume that GoldCoins have currency identifier 3. Doing this takes 29 bytes:

1. [Transaction version](#field-transaction-version) = 1
1. [Transaction type](#field-transaction-type) = 21 (currency trade offer for another Master Protocol-derived currency)
1. [Currency identifier](#field-currency-identifier) = 1 for Mastercoin 
1. [Amount for sale](#field-number-of-coins) = 250,000,000 (2.50000000 Mastercoins) 
1. [Currency identifier desired](#field-currency-identifier) = 3 for GoldCoin 
1. [Amount of GoldCoins desired](#field-number-of-coins) = 5,000,000,000 (50.00000000 GoldCoins)
1. [Action](#field-sell-offer-sub-action) = 1 (New offer)

#### Change an Offer to Sell Master Protocol-Derived Coins for Another Master Protocol-Derived Currency

An offer to sell coins can be changed by using Action = 2 (Update) until either: there are valid corresponding purchase offers for the whole amount offered, or the sell offer is canceled.

The change will apply to the balance that has not yet been accepted. It's your responsibility to determine if the update was successful and how many coins were purchased before the update took effect.

The amount reserved from the available balance for this address will be adjusted to reflect the new amount for sale.

Say you decide you want to change an offer, e.g. the number of coins you are offering for sale, or change the asking price. Send the transaction with the new values and the values that are not changing and Action = 2 (Update) before the whole amount offered has been accepted. 

#### Cancel an Offer to Sell Master Protocol-Derived Coins for Another Master Protocol-Derived Currency

A currency sell offer can be canceled by using Action = 3 (Cancel) until the offer has been fully accepted by valid purchase offers. When a sell offer is canceled, the associated coins are no longer reserved.

The cancel will apply to the amount that has not yet been accepted. It's your responsibility to determine if the cancellation was successful and how many coins were not sold.

If you want to cancel an offer, use Action = 3 (Cancel) and send the transaction before the full amount for sale has been accepted.

### Smart Property

The Master Protocol supports creating property tokens to be used for titles, deeds, user-backed currencies, and even shares in a company. Whenever property is created, it gets assigned the next available currency ID, so any property can be bought, sold, transferred, and in the future used for betting, just as other Master Protocol-derived currencies are.

### New Property Creation

Description: Transaction type 50 is used to create a Smart Property token.

Properties are references by the block number and byte offset of the transaction which created them. Block numbers below 131072 are considered special values, and enable the byte offset to be skipped. Special values include 0 for bitcoin, 1 for Mastercoin and 2 for Test Mastercoins.

If creating a title to a house or deed to land, the number of properties should be 1. Don’t set number of properties to 10 for 10 pieces of land – create a new property for each piece of land, since each piece of land inherently has a different value, and they are not interchangeable.

If creating 1,000,000 units of a divisible currency, choose property type 2 and specify 100,000,000,000,000 for the number of properties (1 million divisible to 8 decimal places).

Once property has been created, the creator owns them at the address which broadcast the message creating them.

Say you want to do an initial distribution of 1,000,000 digital tokens for your company “Quantum Miner”. Doing so will use a varying number of bytes, due to the use of a null-terminated string. This example uses 35 bytes:

1. [Transaction version](#field-transaction-version) = 1
1. [Transaction type](#field-transaction-type) = 50
1. [Property Type](#field-property-type) = 1 for indivisible shares
1. [Property Name](#field-string-null-terminated) = “Quantum Miner Shares\0” (21 bytes)
1. [Number Properties](#field-integer-eight-byte) = 100,000,000,000,000 indivisible shares

## Future Transactions
The transactions below are not yet fully defined and therefore are not included in deployments based on this version of the spec. 

### Marking an Address as “Savings”

1. [Transaction version](#field-transaction-version) = 0
1. [Transaction type](#field-transaction-type) = 10
1. [Reversibility period](#field-time-period-in-seconds) = 2,592,000 (30 days) 

Marking an address as savings is PERMANENT and cannot be undone. If an address is marked as savings, the reversibility rules affect not only Mastercoins, but any Mastercoin-derived child currency stored at that address.

When marking an address as savings, the reference payment should point to a “guardian” address authorized to reverse fraudulent transactions. The guardian address should preferably be from an unused offline or paper wallet. The sending address should be the address to be marked as savings.

When a fraudulent transaction is reversed, any pending funds go to the guardian address, rather than going back to the compromised savings address. Also, any funds which remain in the compromised address also go to the guardian wallet.

An address marked as savings can only do simple transfers (transaction type=0). All other transaction types require addresses without a reversibility time window.

### Marking a Savings Address as Compromised

Say you notice that the address you marked as savings has been compromised, and you want to reverse transactions and transfer everything to the guardian address. Doing this takes 4 bytes:

1. [Transaction version](#field-transaction-version) = 0
1. [Transaction type](#field-transaction-type) = 11 for marking a compromised savings address

This transaction must be sent from the guardian address. The reference payment must be to the compromised savings address. Funds from any pending transactions and any remaining funds will then be transferred to the guardian address, both Mastercoins and any currencies derived from Mastercoins.

#### Advantages of the Savings/Guardian Model

The savings/guardian model is intended to allow the user to take extreme precautions against accidental loss of the savings address (for instance, by storing lots of backups, including in the cloud), and extreme precautions against theft of the guardian address. Although reasonable precautions should be taken, if your savings address gets hacked, or the key to your guardian address gets lost or destroyed, the coins can still be recovered. 

This model also facilitates estate planning. You simply give your heir(s) a paper copy to the private key of your savings address, but you keep the guardian address key to yourself. If you die, your heirs can simply transfer the funds out of your savings (they will have to wait for the reversibility period to pass), but they can't steal from you while you are alive since you are the only one with the key to the guardian address and can reverse their transaction if they try.

It should be obvious that anyone parsing Mastercoin transactions for payment should check that the payment is not reversible before completing the transaction!


### Marking an Address as Rate-Limited

Say you want to enforce a spending limit of 1 Mastercoin per Month on one of your addresses. Doing this takes 20 bytes:

1. [Transaction version](#field-transaction-version) = 0
1. [Transaction type](#field-transaction-type) = 12
1. [Currency identifier](#field-currency-identifier) = 1 for Mastercoin 
1. [Spending Limit](#field-number-of-coins) = 100,000,000 (1.00000000 Mastercoins)
1. [Limitation Reset period](#field-time-period-in-seconds) = 2,592,000 (30 days) 

Marking an address as rate-limited only affects the specified currency. Other currencies stored in the address are not rate-limited. The limitation reset period begins once the protected address makes a send. Attempting to send beyond the rate limit results in the maximum send possible under the limit.

When marking an address as rate-limited, the reference payment must point to a “guardian” address authorized to remove the limitation. The guardian address should preferably be from an unused offline or paper wallet. The sending address must be the address to be marked as rate-limited. Note that an address could be marked as savings AND rate limited, with the same or different guardian addresses.

An address marked as rate limited can only do [Simple Send](#simple-send) transactions. All other transaction types require addresses without a rate limitation.

### Removing a rate limitation

Removing the rate limitation above takes 8 bytes:

1. [Transaction version](#field-transaction-version) = 0
1. [Transaction type](#field-transaction-type) = 14
1. [Currency identifier](#field-currency-identifier) = 1 for Mastercoin 

This transaction must be sent from the guardian address in charge of the rate limitation. The reference payment must be to the rate-limited address. Removing the limit affects only the specified currency, and not any other rate-limited currencies stored at that address.

### Registering a Data Stream
(AKA Data Feed)

Say you decide you would like to start publishing the price of Gold in the block chain. Registering your data stream takes a varying number of bytes due to the use of null-terminated strings. This example uses 57 bytes:

1. [Transaction version](#field-transaction-version) = 0
1. [Transaction type](#field-transaction-type) = 30
1. [Parent currency identifier](#field-currency-identifier) = 1 for Mastercoin (the price of Gold will be published in units of Mastercoin)
1. [Category](#field-string-null-terminated) = “Commodities\0” (12 bytes)
1. [Sub-Category](#field-string-null-terminated) = “Metals\0” (7 bytes)
1. [Label](#field-string-null-terminated) = “Gold\0” (5 bytes) (if a second “Gold” is registered in this sub-category, it will be shown as “Gold-2”)
1. [Description/Notes](#field-contact-uri)  = “tinyurl.com/kwejgoig\0” (22 bytes) (Please save space in the block chain by linking to your description!)
1. [Display Multiplier](#field-integer-four-byte) = 10,000 (if the ticker publishes 0.00150000, the price of an ounce of gold is currently 15.0000 Mastercoins

The reference payment must be to the bitcoin address which will be publishing the data. Only the first payment sent from that address in a given day (as determined by block-chain timestamps) will be considered ticker data. Data published by a ticker should also have an output to the Exodus Address – this will make it easier to find ticker data in the block chain data. The output can be for any amount, but should be above the dust threshold.

Each data stream gets a 4-byte unique identifier, determined by the order in which they were registered. For instance, if your data stream was the third data stream ever registered, your data stream identifier would be 3.

Since anyone can cheaply register a data stream, and thereby create categories and subcategories, we can assume that there will be a lot of noise. Anyone writing code to display data stream categories should note which data streams are the most actively used, and order categories and subcategories by descending activity, thereby pushing unused categories to the bottom of the list. 

If you ever need to change the description/notes for your data stream (for instance, if some poor sport takes down your website), simply re-register it from the same address with the same category, subcategory, and label. When re-registering, you can also change the ticker address by choosing a different address for the reference payment (for instance, if your ticker address gets compromised), or change the display multiplier.
**How to destroy a data stream?**

## Offering a Bet

Say you want to use USDCoins (another hypothetical currency derived from Mastercoin, each USDCoin being worth one U.S. Dollar) to bet $200 that the gold ticker will not rise above 20 Mastercoins/Ounce in the next 30 days at 2:1 odds. For the sake of example, we will assume that USDCoins have currency identifier 5. Creating this bet takes 36 bytes:

1. [Transaction version](#field-transaction-version) = 0
1. Transaction type = 40 for creating a bet offer (32-bit unsigned integer, 4 bytes)
2. Bet Currency identifier = 5 for USDCoin (32-bit unsigned integer, 4 bytes)
3. Data Stream identifier = 3 for the Gold ticker, per our data stream example (32-bit unsigned integer, 4 bytes)
4. Bet Type = 35 for “Will not exceed on or before” (See table below) (16-bit unsigned integer, 2 bytes)
5. Bet threshold (Non-CFDs only) = 200,000 (0.00200000 BTC, which equates to a ticker value of 20 per our data stream example) **OR** Leverage (CFDs only) = 65536 (1x leverage) (32-bit unsigned integer, 4 bytes)
6. Days out = 30 (16-bit unsigned integer, 2 bytes)
7. Amount of wager = 20,000,000,000 (200.00000000 USDCoins) (64-bit unsigned integer, 8 bytes)
8. Amount of counter-wager = 10,000,000,000 (100.00000000 USDCoins) (64-bit unsigned integer, 8 bytes)

Since this bet is not a CFD (described later) "bet threshold" is used rather than "leverage".

By offering $200 against $100, the desired 2:1 odds are implied. Since one address might want to have multiple similar wagers, it is not possible to change a bet (you must cancel and then broadcast a new bet). To cancel your bet, rebroadcast it with all the same data except set the amount of wager to zero.

**Table of Bet Types**

<table>
    <tr>
        <td>0</td>
		<td>Will equal on</td>
		<td>32</td>
		<td>Will equal on or before</td>
    </tr>
	<tr>
        <td>1</td>
		<td>Will not equal on</td>
		<td>33</td>
		<td>Will not equal on or before</td>
    </tr>
	<tr>
        <td>2</td>
		<td>Will exceed on</td>
		<td>34</td>
		<td>Will exceed on or before</td>
    </tr>
	<tr>
        <td>3</td>
		<td>Will not exceed on</td>
		<td>35</td>
		<td>Will not exceed on or before</td>
    </tr>
	<tr>
        <td>4</td>
		<td>Will be below on</td>
		<td>36</td>
		<td>Will be below on or before</td>
    </tr>
	<tr>
        <td>5</td>
		<td>Will not be below on</td>
		<td>37</td>
		<td>Will not be below on or before</td>
    </tr>
    </tr>
	<tr>
        <td>6</td>
		<td>Bullish Contract for Difference</td>
		<td></td>
		<td></td>
    </tr>
    </tr>
	<tr>
        <td>7</td>
		<td>Bearish Contract for Difference</td>
		<td></td>
		<td></td>
    </tr>
</table>

A "Contract for Difference" (CFD) allows a bettor to temporarily gain bullish or bearish exposure to a price movement, in direct proportion to that movement. A bettor who creates a bullish CFD on Gold with 1x leverage (65536) will receive 10% of the counter-wager funds if Gold rises 10% during the period of the bet. If instead Gold falls 10%, the bettor loses 10% of his own money at stake. As with normal bets, 0.5% of the total pot goes to the creator of the data stream before winnings are determined.

CFD bets store "leverage" in place of the data used by "bet threshold" in other bet types. If a bettor prefers that a 10% price movement means a 20% gain or loss, they may select 2x leverage (65536\*2=131072). Similarly, a 10% price movement could mean a 5% gain or loss using 0.5x leverage (65536\*0.5 = 32768). Just as with normal bets, a CFD bettor can "sweeten the deal" by offering better odds (a lower counter-wager amount). High-leverage bets or big price movements could result in a winnings calculation higher than the amount at stake, in which case the winner simply gets the entire pot. 

## Accepting a Bet

Say you see a bet which you would like to accept. Simply publish the inverse bet with matching odds and the same end date, and the Master Protocol will match them automatically (that is, everyone parsing Mastercoin data will mark both bets as accepted). Here is what a bet matching our last example published 5 days later (with 25 days to go) would look like:

1. [Transaction version](#field-transaction-version) = 0
1. Transaction type = 40 for creating a bet offer (32-bit unsigned integer, 4 bytes)
2. Bet Currency identifier = 5 for USDCoin (32-bit unsigned integer, 4 bytes)
3. Data Stream identifier = 3 for the Gold ticker, per our data stream example (32-bit unsigned integer, 4 bytes)
4. Bet Type = 34 for “Will exceed on or before” (See table above) (16-bit unsigned integer, 2 bytes)
5. Bet threshold (Non-CFDs only) = 200,000 (0.00200000 BTC, which equates to a ticker value of 20 per our data stream example) **OR** Leverage (CFDs only) = 65536 (1x leverage) (32-bit unsigned integer, 4 bytes)
6. Days out = 25 (16-bit unsigned integer, 2 bytes)
7. Amount of wager = 5,000,000,000 (50.00000000 USDCoins) (64-bit unsigned integer, 8 bytes)
8. Amount of counter-wager = 10,000,000,000 (100.00000000 USDCoins) (64-bit unsigned integer, 8 bytes)



Note that this bet will be matched against only half of the previous example, because while the odds match (2:1 vs. 1:2), the amount of this bet is for less. This bet is only for $50, so would only win $100 if they win, as opposed to the full $200. Once the bets are matched, the first bet still has $100 available for someone else to bet $50 against.

Once GoldCoins reach a value of 20 or the bet deadline passes, the bet winner gets 99.5% of the money at stake. The other 0.5% goes to the creator of the data stream. 

## Pay Dividends (Send All)

Say your company has made a huge profit and wishes to pay 1000 MSC evenly distributed among the holders of Quantum Miner digital tokens. Doing so will use 20 bytes:

1. [Transaction version](#field-transaction-version) = 0
1. Transaction type = 1 for "send all" (32-bit unsigned integer, 4 bytes)
2. Currency identifier = 1 for Mastercoin (32-bit unsigned integer, 4 bytes)
3. Amount to transfer = 100,000,000,000 (1000.00000000 Mastercoins) (64-bit unsigned integer, 8 bytes, should not exceed number owned, but if it does, assume user is transferring all of them)
4. Currency ID of Payees = 6 for Quantum Miner Shares (32-bit unsigned integer, 4 bytes)

Note that this transaction is very similar to "simple send", with just one extra field. The protocol will split up the 1000 MSC among the shareholders, according to how many shares they have. 

Another use-case for this transaction type would be a giveaway, where someone wants to raise interest in their new coin or property by giving some away to everyone who owns (for instance) Mastercoins.


## Listing Something for Sale

Say you want to sell a Bible for 0.001 Mastercoins. Creating a sell offer will use a variable number of bytes due to the use of null-terminated strings:

1. [Transaction version](#field-transaction-version) = 0
1. Transaction type = 60 for sale listing  (32-bit unsigned integer, 4 bytes)
2. Currency identifier of price = 1 for Mastercoin (32-bit unsigned integer, 4 bytes)
3. Desired price = 100,000 (0.00100000 Mastercoins) (64-bit unsigned integer, 8 bytes)
4. Item category = "Contraband\0" (11 bytes)
5. Item subcategory = "Forbidden Books\0" (16 bytes)
6. Item title = "Bible, NASB\0" (12 bytes)
7. Description/Notes = “tinyurl.com/kwejgoig\0” (22 bytes) (Please save space in the block chain by linking to your description!)

Every sale offer published by a given address gets a 32-bit "Listing ID" number assigned, which increments for each item offered for sale from that address. We'll assume this is the first item offered for sale from this address (Listing ID=0).

To delist an unsold item, publish the exact same message, but with a price of zero. Sellers should make sure they provide some method of contacting them (for instance, on the listing webpage), so they have a communication channel to help resolve disputes with buyers.


## Initiating a Purchase

Say you see the Bible listed above and wish to purchase it. However, you have no reputation as a buyer, so you want to offer a 10% higher purchase price than what the seller is asking. Starting the purchase process takes 16 bytes:

1. [Transaction version](#field-transaction-version) = 0
1. Transaction type = 61 for Initiate purchase from listing  (32-bit unsigned integer, 4 bytes)
2. Listing ID = 0 (the ID for the listing above) (32-bit unsigned integer, 4 bytes)
3. Time limit = 259,200 seconds (3 days) (32-bit unsigned integer, 4 bytes) 
4. Price multiplier = 110% (65536*1.1 = 72090) (32-bit unsigned integer, 4 bytes) 

The reference address should point to the address which listed the Bible for sale. The seller now has 3 days to accept this buyer before the offer expires. The buyer's money is now locked in escrow until their offer expires or the purchase is complete.

The price multiplier can also be used to offer less than the suggested price. This may be viable for an established buyer and/or a stale listing.


## Accepting a Buyer

If the buyer offers a bad price, has a bad reputation, or has no reputation, then you may not wish to do business with them. If you see an offer that you like, the message to accept the offer takes X bytes:

1. [Transaction version](#field-transaction-version) = 0
1. Transaction type = 62 for Accept buyer offer (32-bit unsigned integer, 4 bytes)
2. Which buyer = 2 (3rd offer received) (16-bit unsigned integer, 2 bytes) 

Once a buyer has been accepted, the seller may ship the Bible.


## Leaving Feedback

Once a buyer has been accepted, they may release funds held in escrow (or destroy those funds) and leave feedback. To do so takes a variable number of bytes due to the use of a null-terminated string:

1. [Transaction version](#field-transaction-version) = 0
1. Transaction type = 63 for Release Funds and Leave Feedback (32-bit unsigned integer, 4 bytes)
2. Listind ID = 0 (the ID for the listing above) (32-bit unsigned integer, 4 bytes)
3. Percentage of funds to release = 105% (65536*1.05 68813) (32-bit unsigned integer, 4 bytes)
4. Text feedback = “tinyurl.com/kwejgoig\0” (22 bytes) (Please save space in the block chain by linking to your feedback!)

The reference address should point to the address which listed the Bible for sale. Funds which are not released are permanently destroyed. Specifying more than 100% signifies an additional tip beyond the funds held in escrow. Funds are released automatically after 60 days if the buyer never leaves feedback. In addition to the text feedback, each transaction gets "1 star" to "5 stars" based on the following criteria:

* 1 Star: All funds destroyed (very unhappy customer)
* 2 Stars: Some funds destroyed
* 3 Stars: No funds destroyed, no tip
* 4 Stars: Tip < 10%
* 5 Stars: Tip >= 10%

In order to avoid people gaming the reputation system, some coins must be destroyed with every purchase. The percentage of coins destroyed goes down with each new purchase. The percentage is calculated as 2\*(value of this purchase) / (value of all purchases, including this one). Note that this formula causes 50% of the coins from the first purchase to be destroyed.

# Escrow-Backed User Currencies (experimental proposed feature)

The most important and also the most controversial feature (at least the escrow backed part) of the Master Protocol is the built-in support for users to create their own currencies out of existing Mastercoins. For the purposes of demonstrating how user currencies will work, we will use an example currency called “GoldCoins”, which are intended to track the value of one ounce of gold, and which may be stored, transferred, bought, and sold similarly to Mastercoins.

## Stability Concept

So how do we drive the value of these GoldCoins to their target value, when demand for them may surge and decline? The price of GoldCoins is decided by the balance of supply and demand. Since we can’t control the demand for GoldCoins, we must control the supply. The key to accomplishing this is to use an escrow fund which holds Mastercoins, as shown below:

![Mastercoin Protocol Layers](images/stability.png) 

The escrow fund operates like a battery on the power grid, charging when there is excess energy then discharging where there isn't enough. When there are too few GoldCoins (GoldCoin price is too high), the escrow fund releases new GoldCoins, and the escrow-battery “charges” by holding Mastercoins in escrow. When there are too many GoldCoins (GoldCoin price is too low), the escrow fund purchases some of the excess GoldCoins, thereby “discharging” the escrow-battery as it releases the stored Mastercoins.

## New Currency Creation

Say you want to create the GoldCoin currency described above, using the Gold data stream we defined. Doing so will use a varying number of bytes, due to the use of a null-terminated string. This example uses 38 bytes:

1. [Transaction version](#field-transaction-version) = 0
1. Transaction type = 100 for creating a new child currency (32-bit unsigned integer, 4 bytes)
1. Data Stream identifier = 3 for the Gold ticker, per our data stream example (32-bit unsigned integer, 4 bytes)
1. Escrow fund delay = 4 for 4 days (see below) (8-bit unsigned integer, 1 byte)
1. Escrow fund aggression factor = 1,000,000 for 1% (See below) (32-bit unsigned integer, 4 bytes)
1. Currency Name = “GoldCoin\0” (9 bytes)
1. Escrow Fund Initial Size = 100,000,000,000 for 1,000 Mastercoins (64-bit unsigned integer, 8 bytes, causes 1,000 Mastercoins to be debited from the currency creator and credited to the escrow fund. This number should not exceed the amount owned by the creator, but if it does, assume they are crediting all their Mastercoins to the escrow fund)
1. Escrow Fund Minimum Size = 99,000,000 for 99% (32-bit unsigned integer, 4 bytes, if the escrow fund value is ever less than 99% of all GoldCoins, the currency is dissolved and the escrow fund is distributed to GoldCoin holders who would take a 1% loss)
1. Sale/Transfer Penalty = 100,000 for 0.1% (32-bit unsigned integer, 4 bytes, any time GoldCoins are sold or transferred, 0.1% are destroyed, which improves the health of the escrow fund)


As with properties, currencies are referred to in general by the block number and byte offset within the block of the transaction which created them. 

The currency held in escrow is the parent currency of the data stream. In this example it is Mastercoins, but it could also be any currency derived from Mastercoins. For instance, GoldCoins could later be held in escrow to support a currency whose data stream uses GoldCoins as a parent currency.

The escrow fund delay of 4 days means that the price of GoldCoins must be too high (or too low) for 4 days in a row before the escrow fund will take any action.

The escrow fund aggression factor determines how aggressively the escrow fund corrects the price of GoldCoins when their price diverges from their target. An escrow fund with aggression factor of 0 will never take any action. If the aggression factor is 100%, the escrow fund will take the maximum possible action (buying every GoldCoin for sale above the target price, or selling new GoldCoins to every buyer below the target price).

In the case of a 1% aggression factor, the escrow fund's first action will be to fix 1% of the error. If the error the next day is still in the same direction, the escrow fund will fix 2% of the error, then 3% the next day, and so on until it reaches 100% or the error changes direction. Once the error changes its direction, the escrow fund has done its job and it starts counting again from zero.

The fields Escrow Fund Initial Size, Escrow Fund Minimum Size, and  Sale/Transfer Penalty were added in response to the “bytemaster/d’aniel attack”, which becomes possible once malicious actors are able to short these currencies. The attack only works on currencies with underfunded escrows, and consists of a malicious actor creating a competing GoldCoin with a healthy escrow fund, which the market would presumably prefer over the GoldCoin with the unhealthy escrow fund. The malicious actor could then profit by shorting the unhealthy GoldCoin until people panicked and fled for the healthy version. More information about unhealthy escrow funds can be found in the next section.

## Unhealthy Escrow Funds

What if the price of Mastercoins falls 95%, and the value of the escrow fund is now only 5% of the target value of all GoldCoins? Using the battery analogy, this escrow fund now has less “charge” and is therefore less capable of intervening to correct prices.

If the currency creator had set the minimum escrow size to 100% the escrow fund would never get into this situation because it would simply dissolve and pay out to currency stakeholders as soon as the escrow fund value dropped to parity, with zero or minimal losses. For currencies which are set up to allow continued operation once unhealthy, the protocol responds by adjusting the aggression factor accordingly. In the example of GoldCoins backed by only 5% given above, the 1% aggression factor would be multiplied by 5% to get 0.05%, meaning that the adjustments will be of much smaller magnitude, and it will take a lot longer to get to 100% aggression.

Note that escrow funds holding funds worth more than their currency do not get more aggressive. That is, if the GoldCoins escrow fund is worth twice the value of all GoldCoins in existence, the aggression factor is still 1%.

## Maintaining Escrow Fund Health

Given a reasonably stable Mastercoin, escrow funds should generally grow healthier over time. Our GoldCoin escrow fund, when it does act, is buying GoldCoins when they are cheap, and selling them when they are expensive. Thus it will generally tend to make a profit, and the Mastercoins held by the escrow fund will grow. The larger the escrow fund, the lower the chance of the currency failing to maintain its value. Additionally, the currency creator can optionally supply initial escrow funds if desired, and the currency can be tuned to destroy some GoldCoins with every sale or transfer, further increasing escrow fund health.

When an escrow fund is unhealthy, lowering the aggression factor makes the escrow fund take more profitable trades, which increases the likelihood of recovery. For instance, if it is buying excess GoldCoins, the cheapest 0.05% can be purchased at a better average price than the cheapest 1% on the market.

Escrow funds should generally be tuned to act slowly. This will allow arbitrage traders to do the heavy lifting, as the knowledge that the escrow fund will eventually get the price back to the target makes for a self-fulfilling prophecy when traders act on that knowledge. If the escrow fund acts too quickly, it loses money when the bitcoin version of a security leads the real-world version, as would happen if someone was engaging in insider trading anonymously using the bitcoin version.



# Appendix A – Storing Mastercoin data in the blockchain 
By Zathras, Copyright © 2013 The Mastercoin Foundation 

The following appendix serves to detail the different approaches to storing Mastercoin transaction data in the Bitcoin blockchain along with their validity requirements and use cases. 

This appendix will not discuss the varying types of Mastercoin transaction or what the transaction data contains (these are defined in the main body of the Mastercoin specification) and will focus solely on transaction data storage.
 
For the purposes of a simplified overview, parties wishing to develop Mastercoin software should support the decoding of both Class A and Class B transactions, but only need support encoding of Class B transactions. 

## Class A transactions (also known as the ‘original’ method) 

Class A transactions were the first class of Mastercoin transaction and store data in the blockchain by utilizing fake Bitcoin addresses to encode transaction data. 

The transaction data is encoded into said fake Bitcoin address which is then used as an output in a single Bitcoin transaction satisfying the following requirements: 

* Has a single or the largest input signed by the sending address
* Has an output for the recipient address (the 'reference' address)
* Has an output for the exodus address
* Has an output for the encoded fake address (the 'data' address)
* Has all output values above the 'dust' threshold (currently 0.00005460 BTC) and preferable be equal. 
* Has exactly two non-Exodus outputs (one of which must be the data address) with a value equal to the Exodus output and/or has exactly one output with a sequence number +1 of the data address for reference output identification
* Additional outputs are permitted for the remainder of the input (the 'change' address) 

NOTE: The sequence number for a given address is defined as a 1 -byte integer stored as the first byte of each 'packet'.   Sequence numbers are continuous with 0 following 255 (256=0, 255+1=0). 

NOTE: Should a transaction result in an edge case that provides conflicting reference address values for sequence numbers and equal outputs, the reference address identified via equal outputs will take precedence.

As there is no private key associated with these fake addresses they are inherently unspendable.  This creates concerns around blockchain bloat, especially within the UTXO (Unspent Transaction Output) set as each use of a fake address adds an unspent output to the UTXO dataset that will never be redeemed, thus growing (or ‘bloating’) it. 

As the UTXO set is designed to be memory resident it is thus in the interests of Bitcoin to avoid UTXO bloat to minimize the memory requirement for client implementations. 

Class B transactions were developed to address this issue by using provably redeemable outputs.  Class A transactions are thus considered deprecated and are supported for backwards compatibility only.    

NOTE: Class A transactions are restricted to the ‘simple send’ transaction type only.  All other Mastercoin transaction types are supported by Class B transactions only.  Client implementations should utilize Class B for all transaction types, including ‘simple send’. 

## Class B transactions (also known as the ‘multisig’ method) 

Class B transactions attempt to address the UTXO ‘bloat’ issue by storing data in the blockchain by utilizing ‘1-of-n’ multisignature outputs where one of the signatories is always the sender.   

By adopting a ‘1-of-n’ approach (credit Tachikoma @ bitcointalk) we can increase n to the number of packets (public keys) needed to store the transaction data while maintaining the ability of the sender to redeem the output.  

NOTE: The reference client currently supports a maximum value of 3 for n.  As one signatory must be the sender for redemption purposes, there is a current limit of 2 data packets per output.  A number of multisig outputs can be combined to increase the space available for transaction data as required.  On decoding all Mastercoin packets from all multisig outputs are ordered via their sequence number and evaluated as a continuous data stream. 

Transaction data is encoded into one or a number of compressed public keys which are obfuscated and then have their last byte manipulated to form a valid ECDSA point.  These compressed public keys then can be included as signatories in a multisig output. 

The size of a Mastercoin packet in a compressed public key is thus 31 bytes (33 bytes minus the first and last bytes for the key identifier (02) and ECDSA manipulation byte).   The Mastercoin packet reserves the first byte for the sequence number, providing a total of 30 bytes per packet for Mastercoin transaction data.  The range of sequence numbers in a Class B transaction is 1 to 255, providing for a total 7,650 bytes maximum actual transaction data storage per Mastercoin Class B transaction. 

Sequence numbers are again used to order the packets (again first byte of the packet), however as we no longer need to use sequence numbers to identify the recipient (reference) address we are able to start the sequence at one (we do not start the sequence at zero due to our need for a positive sequence number in obfuscation). 

Obfuscation is performed by SHA256 hashing the sender's address S times (where S is the sequence number) and taking the first 31 bytes of the resulting hash and XORing with the 31 byte Mastercoin  packet.  Multiple SHA256 passes are performed against an uppercase hex representation of the previous hash.  
                
EXAMPLE:  The following provides example output for an obfuscated Mastercoin packet (where and XX is the last byte reserved for ECDSA point validity manipulation):
<table>
<tr>
    <td>REFERENCE ADDRESS</td><td>{1CdighsfdfRcj4ytQSskZgQXbUEamuMUNF }</td> 
</tr><tr>
    <td>SHA256 HASH (S TIMES) OF ADDRESS</td><td>{1D9A3DE5C2E22BF89A1E41E6FEDAB54582F8A0C3AE14394A59366293DD130C }59</td>
</tr><tr>
    <td>CLEARTEXT MASTERCOIN PACKET</td><td>02{ 0100000000000000010000000002faf0800000000000000000000000000000 }XX </td>
</tr><tr>
    <td>OBFUSCATED MASTERCOIN PACKET</td><td>02{ 1C9A3DE5C2E22BF89B1E41E6FED84FB502F8A0C3AE14394A59366293DD130C }XX </td>
</tr>
</table>

Once the obfuscated Mastercoin packet is prepared, the key identifier (02) is prefixed and a random byte compressed public key is then run across a check to ensure the key is a valid ECDSA point.  If the key fails this check, the last byte is simply rotated with a different random byte and tested again until the key forms a valid ECDSA point. 

![Mastercoin Protocol Layers](images/classb_obfuscated.png) 

These compressed public key 'packets' can then be included in one or multiple OP_CHECKMULTISIG output along with the senders public key.  A single transaction must be constructed satisfying the following requirements: 
* Has a single or the largest input signed by the sending address
* Has an output for the recipient address (the 'reference' address)
* Has an output for the exodus address
* Has one or more 1-of-n OP_CHECKMULTISIG outputs each containing at least two compressed public keys, one of which must be the senders address
* Has all output values above the 'dust' threshold (the threshold is higher for multisig outputs)  
* An additional output is permitted for the remainder of the input (the 'change' address) however this output must be addressed to the sending address   



# Appendix B – Regulatory and Legal Complience - Know Your Jurisdiction

It should be clear by now that the Master Protocol can be used for activities that may be regulated or even prohibited in certain jurisdictions. Anyone working on an implementation of the Master Protocol should be very careful to warn users to know and understand the regulatory environment of their jurisdiction and country of residence in order to not break any laws. It is up to the user to know the laws of their country, and not (for instance) engage in sports betting in a jurisdiction / country where sports betting is not a legal activity.

Also, the contributors to this open source specification are not securities experts, and offer no advice or counsel on how to properly comply with securities or other regulations. This protocol is presented as a open source tool on which others can implement clients and build innovative services for the benefit of others. 

That said, stable distributed currencies / smart property and the other features of this protocol will be incredibly useful in a huge number of legal applications, and even modest success of this protocol could allow early adopters (and even those who simply hold bitcoins) to greatly benefit. The Master Protocol and Mastercoins, are just neutral tools, capable of being used for good or for evil. We urge our early adopters to consider how they may use Mastercoin for good, and if they gain from its adoption, to use those funds for good. It will take a lot of work to make the good, outshine bad actors.



# Appendix C – Financial Disclaimer

Placing your funds in experimental currencies is really, absurdly risky. This paper is not investment advice, and anyone predicting what will happen with experimental currencies such as those described here is indulging in the wildest sort of speculation, and that includes the speculations in the previous appendix.

Please consult your financial adviser before purchasing Mastercoins or other digital tokens (hint: they will probably tell you to RUN and not look back unless you assure them that it is money you are totally prepared to lose).

Anyone who puts their rent money or life savings into an experiment of this type is very unwise, and is risking financial ruin from this or similarly other risky enterprise.

# Appendix D - Webservice verification API

One of the large differences between Mastercoin and Bitcoin is that there is no reference implementation available for Mastercoin which you can use to test your own implementation. The official spec, the document you hopefully just read, is open for interpretation. This makes it very difficult to make sure every implementation processes transactions the same. In order to make it easier to compare implementations and spot discrepancies every webbased Mastercoin service should ideally implement the following API calls.

**GET /mastercoin_verify/addresses?currency_id=#currency_id#**

```json
[{address: 1KZmDQGzGJWYmPP9X3b7TA9dY91KBXgaG4, balance: 20.1}, {address: 1Q1sFqsi8S5DxV5hz6sWLamGBp9To93iG7, balance: 3.1}, etc..]
```

You supply this URL a currency_id, initially 1 or 2, and it should return an JSON array of objects with two keys: address and balance. This will be a quick way to spot differences between implementations. If an other returns a different balance for two implementations a second call can be used to spot the offending transaction.

**GET /mastercoin_verify/transactions/#address#?currency_id=#currency_id#**

```json
{address: 1KZmDQGzGJWYmPP9X3b7TA9dY91KBXgaG4, transactions: [{tx_hash: 5f01def181b761f1d03bcd20590c5729a47b11c68955b364add9253d7aec5eb9, valid: true, accepted_amount: nil, bought_amount: nil}, {tx_hash: 130c5175d4f3e9add03bd1d115a87b26e613293fbe3815b970f8fc830f018ebc, valid: false, accepted_amount: nil, bought_amount: nil}, etc..]} 
```

This URL takes an address and currency_id as arguments and should return an JSON object with an address and a transactions key for this given address. The transactions key should have an array of all transactions for this address and whether this implementation considers a given transaction valid or not. 

In all likeliness this will capture most of the discrepancies. If this doesn't proof enough we can supply addional information like the amount transferred per transaction in the future.

For Simple Send transactions accepted_amount and bought_amount can be null values. These values are only used for Distributed Exchange transactions. The accepted amount should contain the amount that was accepted when a Purchase Offer got added to a block.

Example:

User A has a Selling Offer for 5 MSC. User B sends out a Purchase Offer for 8. It gets added to a block but since User A only had a Selling Offer for 5 the Accepted Amount for User B's Purchase Offer is now 5. 

The bought amount is the total amount a user actually spends on an open Purchase Offer.

Example: 

User B has a valid Purchase Offer to buy 5 MSC from User A. He sends out a transaction that actually purchases 2 MSC. At that point his Purchase Offer has an bought_amount of 2 MSC. If he decides to sent an other 3 MSC later this values gets updated to 5 MSC.
