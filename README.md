Mastercoin Complete Specification
=================================

vs1.2 (Tons of New Features Edition)

dacoinminster (jr DOT willett AT gmail DOT com)

The Mastercoin Foundation - info@mastercoin.org

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

* Version 0.5 released 1/6/2012 (No packet definitions, overly-complicated currency stabilization)
* Version 0.7 released 7/29/2013 (Preview of 1.0, but without revealing the Exodus Address)
* Version 1.0 released 7/31/2013 (Version used during the fund-raiser)
* Version 1.1 released 9/9/2013 (Smart Property. Also, improvements for easier parsing and better escrow fund health)
* Version 1.2 released 11/11/2013 (Added "Pay Dividend" message, spending limits for savings wallets, contract-for-difference bets, and distributed e-commerce messages. Also added Zathras' new appendix (description of class B and class C methods of storing Mastercoin data.)


Pre-github versions of this document (prior to version 1.2) can be found at https://sites.google.com/site/2ndbtcwpaper/

# Mastercoin Design

The “Mastercoins” protocol layer between the existing bitcoin protocol and users’ currencies is intended to be a base upon which anyone can build their own currency. The software implementing Mastercoins will contain simple tools which will allow anyone to design and release their own currency with their own rules without doing any software development.

## The “Exodus Address”

Perhaps you have heard of the “Genesis Block” which launched the bitcoin protocol. The Mastercoin protocol has a similar starting point in the block chain, called the “Exodus Address” - the bitcoin address from which the first Mastercoins were sold during the month of August 2013. The Exodus Address is: **[1EXoDusjGwvnjZUyKkxZ4UHEf77z6A5S4P](https://blockchain.info/address/1EXoDusjGwvnjZUyKkxZ4UHEf77z6A5S4P)**  

Initial distribution of Mastercoins was essentially a fundraiser to provide money to pay developers to write the software which fully implements the protocol. The distribution was very simple, and proceeded as follows:

1. Anyone who sent bitcoins to the Exodus Address before August 31st, 2013 was recognized by the protocol as owning 100x that number of Mastercoins. For instance, if I sent 100 bitcoins to the Exodus Address before August 31st, my bitcoin address owns 10,000 Mastercoins after August 31st. 
2. Early buyers got additional Mastercoins. In order to encourage adoption momentum, buyers got an additional 10% bonus Mastercoins if they made their purchase a week before the deadline, 20% extra if they purchased two weeks early, and so on, including partial weeks. Thus, if I sent 100 bitcoins to the exodus address 1.5 weeks before August 31st, the protocol recognized my bitcoin address as owning 11,500 Mastercoins (10000 + 15% bonus).
3. Attempts to send funds to the Exodus Address on or after September 1st 2013 (after block #255365) were not considered Mastercoin purchases, and were refunded to the sender.

In the event that a purchase had multiple inputs, the input address contributing the most funds was recognized as owning the Mastercoins.

Note that anyone who purchased Mastercoins also received the same number of “Test Mastercoins” which are being used for testing new features before they are available for use in the Mastercoin protocol.

Initially, the only valid Mastercoin transaction was a “simple send” (defined later in this document), but the additional features described in this document are being implemented, and can be used once they are fully tested.

## Dev Mastercoins

For every 10 Mastercoins sold, an additional “dev Mastercoin” was also created, which are being awarded to the Exodus Address slowly over the years following the fundraiser. These delayed Mastercoins will ensure that we (the Mastercoin Foundation) have funding to complete the features desired by users. The reward is be structured so that we receive 50% of the reward by one year after the initial sale, 75% by a year later, 87.5% by a year later, and so on:

![Reward Mastercoins](images/reward-mastercoin-formula.png)

## Hiding Mastercoin Protocol Data in the Block Chain

Bitcoin has some little-known advanced features (such as scripting) which many people imagine will enable it to perform fancy new tricks someday. Mastercoin uses exactly NONE of those advanced features, because support for them is not guaranteed in the future, and Mastercoin doesn't need them anyway.

Mastercoin was originally specified to hide data in the block chain using fake bitcoin addresses (Class A), but we've since come up with a more blockchain friendly method which hides data in a bitcoin multi-signature transaction (Class B). Once bitcoin miners start supporting the new OP_RETURN opcode, we'll be able to use that opcode to make Mastercoin data completely prunable (Class C).

The technical details for both Class A and Class B transactions can be found in Appendix A. 

## Special Considerations to Avoid Invalid Transactions

Not every wallet lets you choose which address bitcoins come from when you make a payment, and Mastercoin transactions must all come from the address which holds the Mastercoins. If a wallet contains bitcoins stored in multiple addresses, the wallet must first consolidate the bitcoins by sending ALL of them to the address which is going to initiate a Mastercoin transaction. Then, any Mastercoin-related bitcoin transactions will be sent from that address.

Wallets which do not allow you to consolidate to one address and send from that address (such as online web wallet providers) will not work for Mastercoin unless they are modified to do so. For this reason, **attempting to purchase Mastercoins from an online web wallet will likely result in the permanent loss of those Mastercoins.**


## Transferring Mastercoins (Simple Send)

Say you want to transfer 1 Mastercoin to another address. Only 16 bytes are needed. The data stored is:

1. Transaction type = 0 for simple send (32-bit unsigned integer, 4 bytes)
2. Currency identifier = 1  for Mastercoin (32-bit unsigned integer, 4 bytes)
3. Amount to transfer = 100,000,000 (1.00000000 Mastercoins) (64-bit unsigned integer, 8 bytes, should not exceed number owned, but if it does, assume user is transferring all of them)


Note that the amount to transfer is multiplied by 100,000,000 before it is stored, which allows for Mastercoins to be sent with the same precision as bitcoins (eight decimal places). The reference payment (described earlier) determines the address receiving the Mastercoins.

Note that if the transfer comes from an address which has been marked as “Savings”, there is a time window in which the transfer can be undone. Otherwise Mastercoin transactions are not reversible.



## Marking an Address as “Savings”

Say you want to back up your savings wallet in the cloud, but if someone manages to hack into it, you want transactions out of that wallet to be reversible for up to 30 days. Doing this takes 8 bytes:

1. Transaction type = 10 for marking savings (32-bit unsigned integer, 4 bytes)
2. Reversibility period = 2,592,000 seconds (30 days) (32-bit unsigned integer, 4 bytes) 

The maximum reversibility period is 365 days (1,892,160,000 seconds) to avoid accidents. Marking an address as savings is PERMANENT and cannot be undone. If an address is marked as savings, the reversibility rules affect not only Mastercoins, but any Mastercoin-derived child currency stored at that address.

When marking an address as savings, the reference payment should point to a “guardian” address authorized to reverse fraudulent transactions. The guardian address should preferably be from an unused offline or paper wallet. The sending address should be the address to be marked as savings.

When a fraudulent transaction is reversed, any pending funds go to the guardian address, rather than going back to the compromised savings address. Also, any funds which remain in the compromised address also go to the guardian wallet.

An address marked as savings can only do simple transfers (transaction type=0). All other transaction types require addresses without a reversibility time window.


## Marking a Savings Address as Compromised

Say you notice that the address you marked as savings has been compromised, and you want to reverse transactions and transfer everything to the guardian address. Doing this takes 4 bytes:

1. Transaction type = 11 for marking a compromised savings address (32-bit unsigned integer, 4 bytes)

This transaction must be sent from the guardian address. The reference payment must be to the compromised savings address. Funds from any pending transactions and any remaining funds will then be transferred to the guardian address, both Mastercoins and any currencies derived from Mastercoins.


## Advantages of the Savings/Guardian Model

The savings/guardian model is intended to allow the user to take extreme precautions against accidental loss of the savings address (for instance, by storing lots of backups, including in the cloud), and extreme precautions against theft of the guardian address. Although reasonable precautions should be taken, if your savings address gets hacked, or the key to your guardian address gets lost or destroyed, the coins can still be recovered. 

This model also facilitates estate planning. You simply give your heir(s) a paper copy to the private key of your savings address, but you keep the guardian address key to yourself. If you die, your heirs can simply transfer the funds out of your savings (they will have to wait for the reversibility period to pass), but they can't steal from you while you are alive since you are the only one with the key to the guardian address and can reverse their transaction if they try.

It should be obvious that anyone parsing Mastercoin transactions for payment should check that the payment is not reversible before completing the transaction!


## Marking an Address as Rate-Limited

Say you want to enforce a spending limit of 1 Mastercoin per Month on one of your addresses. Doing this takes 20 bytes:

1. Transaction type = 12 for rate limitation (32-bit unsigned integer, 4 bytes)
2. Currency identifier = 1 for Mastercoin (32-bit unsigned integer, 4 bytes)
3. Spending Limit = 100,000,000 (1.00000000 Mastercoins) (64-bit unsigned integer, 8 bytes)
4. Limitation Reset period = 2,592,000 seconds (30 days) (32-bit unsigned integer, 4 bytes) 

Marking an address as rate-limited only affects the specified currency. Other currencies stored in the address are not rate-limited. The limitation reset period begins once the protected address makes a send. Attempting to send beyond the rate limit results in the maximum send possible under the limit.

When marking an address as rate-limited, the reference payment should point to a “guardian” address authorized to remove the limitation. The guardian address should preferably be from an unused offline or paper wallet. The sending address should be the address to be marked as rate-limited. Note that an address could be marked as savings AND rate limited, with the same or different guardian addresses.

An address marked as rate limited can only do simple transfers (transaction type=0). All other transaction types require addresses without a rate limitation.


## Removing a rate limitation

Removing the rate limitation above takes 8 bytes:

1. Transaction type = 14 for removing rate limitation (32-bit unsigned integer, 4 bytes)
2. Currency identifier = 1 for Mastercoin (32-bit unsigned integer, 4 bytes)

This transaction must be sent from the guardian address in charge of the rate limitation. The reference payment must be to the rate-limited address. Removing the limit only affects the specified currency, and not any other rate-limited currencies stored at that address.


## Selling Mastercoins for Bitcoins

Say you want to publish an offer to sell 1.5 Mastercoins for 1000 bitcoins. Doing this takes 33 bytes:

1. Transaction type = 20 for currency trade offer for bitcoins (32-bit unsigned integer, 4 bytes)
2. Currency identifier for sale = 1 for Mastercoin (32-bit unsigned integer, 4 bytes)
3. Amount for sale = 150,000,000 (1.50000000 Mastercoins) (64-bit unsigned integer, 8 bytes, should not exceed the number owned, but if it does,  assume the user is selling all of them)
4. Amount of bitcoins desired = 100,000,000,000 (1000.00000000 bitcoins) (64-bit unsigned integer, 8 bytes)
5. Time limit = 10 (10 blocks in which to send payment after counter-party accepts these terms) (8-bit unsigned integer, 1 byte)
6. Minimum bitcoin transaction fee = 10,000,000 (require that the buyer pay a hefty 0.1 BTC transaction fee to the miner, discouraging fake offers) (64-bit unsigned integer, 8 bytes)

## Changing an Offer

Say you decide you want to change the number of coins you are offering for sale, or change the asking price. Simply re-send the offer with the new details. If your change gets into the block chain before someone accepts your old offer, your offer has been updated.

If you decide you want to cancel an offer, simply send the offer again, but enter the number of coins for sale as zero.

## Purchasing Mastercoins with Bitcoins

Say you see an offer such as the one listed above, and wish to initiate a purchase of those Mastercoins. Doing so takes 16 bytes:

1. Transaction type = 22 for accepting currency trade offer (32-bit unsigned integer, 4 bytes)
2. Currency identifier you are purchasing = 1 for Mastercoin (32-bit unsigned integer, 4 bytes)
3. Amount you are purchasing = 130,000,000 (1.30000000 Mastercoins) (64-bit unsigned integer, 8 bytes, should not exceed number available for sale, but if it does, assume buyer is purchasing all of them)


The reference address should point to the seller's address, to identify whose offer you are accepting.

Note: Make sure your total expenditures on bitcoin transaction fees while accepting the purchase meet the minimum fee requested!

You will need to send the appropriate amount of bitcoins before the time limit expires to complete the purchase. Note that you must send the bitcoins from the same address which initiated the purchase. If you send less than the correct amount of bitcoins, your purchase will be for that amount. In order to make parsing Mastercoin transactions easier, you must also include an output to the Exodus Address when sending the bitcoins to complete a purchase of Mastercoins. The output can be for any amount, but should be above the dust threshold.

## Selling Mastercoins for Other Mastercoin-Derived Currencies

Say you want to publish an offer to sell 2.5 Mastercoins for 50 GoldCoins (coins which each represent one ounce of gold, derived from Mastercoins and described later in this document). For the sake of example, we'll assume that GoldCoins have currency identifier 3. Doing this takes 28 bytes:

1. Transaction type = 21 for currency trade offer for another Mastercoin-derived currency (32-bit unsigned integer, 4 bytes)
2. Currency identifier for sale = 1 for Mastercoin (32-bit unsigned integer, 4 bytes)
3. Amount for sale = 250,000,000 (2.50000000 Mastercoins) (64-bit unsigned integer, 8 bytes, should not exceed the number owned, but if it does,  assume the user is selling all of them)
4. Currency identifier desired = 3 for GoldCoin (32-bit unsigned integer, 4 bytes)
5. Amount of GoldCoins desired = 5,000,000,000 (50.00000000 GoldCoins) (64-bit unsigned integer, 8 bytes)

To accept the offer above, simply publish the same message type with an inverse offer (selling Goldcoins for Mastercoins) at a price which matches or beats the seller's price. The protocol simply finds orders that match and the coins from matching orders are considered transfered at the price specified by the earlier of the two offers.

Note that when only some coins are purchased, the rest are still for sale with the same terms.

## Registering a Data Stream

Say you decide you would like to start publishing the price of Gold in the block chain. Registering your data stream takes a varying number of bytes due to the use of null-terminated strings. This example uses 57 bytes:

1. Transaction type = 30 for registering a data stream (32-bit unsigned integer, 4 bytes)
2. Parent Currency Identifier = 1 for Mastercoin (32-bit unsigned integer, 4 bytes) (Meaning that the price of Gold will be published in units of Mastercoin)
3. Category = “Commodities\0” (12 bytes)
4. Sub-Category = “Metals\0” (7 bytes)
5. Label = “Gold\0” (5 bytes) (if a second “Gold” is registered in this sub-category, it will be shown as “Gold-2”)
6. Description/Notes = “tinyurl.com/kwejgoig\0” (22 bytes) (Please save space in the block chain by linking to your description!)
7. Display Multiplier = 10,000 (if the ticker publishes 0.00150000, the price of an ounce of gold is currently 15.0000 Mastercoins. (32-bit unsigned integer, 4 bytes)


The reference payment should be to the bitcoin address which will be publishing the data. Only the first payment sent from that address in a given day (as determined by block-chain timestamps) will be considered ticker data. Data published by a ticker should also have an output to the Exodus Address – this will make it easier to find ticker data in the block chain data. The output can be for any amount, but should be above the dust threshold.

Each data stream gets a unique identifier, determined by the order in which they were registered. For instance, if your data stream was the third data stream ever registered, your data stream identifier would be 3.

Since anyone can cheaply register a data stream, and thereby create categories and subcategories, we can assume that there will be a lot of noise. Anyone writing code to display data stream categories should note which data streams are the most actively used, and order categories and subcategories by descending activity, thereby pushing unused categories to the bottom of the list. 

If you ever need to change the description/notes for your data stream (for instance, if some poor sport takes down your website), simply re-register it from the same address with the same category, subcategory, and label. When re-registering, you can also change the ticker address by choosing a different address for the reference payment (for instance, if your ticker address gets compromised), or change the display multiplier.

## Offering a Bet

Say you want to use USDCoins (another hypothetical currency derived from Mastercoin, each USDCoin being worth one U.S. Dollar) to bet $200 that the gold ticker will not rise above 20 Mastercoins/Ounce in the next 30 days at 2:1 odds. For the sake of example, we will assume that USDCoins have currency identifier 5. Creating this bet takes 36 bytes:

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

Say you see a bet which you would like to accept. Simply publish the inverse bet with matching odds and the same end date, and the Mastercoin protocol will match them automatically (that is, everyone parsing Mastercoin data will mark both bets as accepted). Here is what a bet matching our last example published 5 days later (with 25 days to go) would look like:

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


## Smart Property

Mastercoin supports creating property tokens to be used for titles, deeds, user-backed currencies, and even shares in a company. Whenever property is created, it gets assigned the next available currency ID, so any property can be bought, sold, transferred, and even used for betting, just like other Mastercoin-derived currencies.


## New Property Creation

Say you want to create a new IPO for your company “Quantum Miner”. Doing so will use a varying number of bytes, due to the use of a null-terminated string. This example uses 37 bytes:

1. Transaction type = 50 for creating new property (32-bit unsigned integer, 4 bytes)
2. Property Type = 1 for indivisible shares (2 is divisible currency) (32-bit unsigned integer, 4 bytes)
3. Property Name = “Quantum Miner Shares\0” (21 bytes)
4. Number Properties = 1,000,000 indivisible shares (64-bit unsigned integer, 8 bytes)

As with data streams, properties are awarded currency identifiers in the order in which they are created. Mastercoin is currency identifier 1 (bitcoin is 0), and Test Mastercoins have currency identifier 2.

If creating a title to a house or deed to land, the number of properties should be 1. Don’t set number of properties to 10 for 10 pieces of land – create a new property for each piece of land, since each piece of land inherently has a different value, and they are not interchangeable.

If creating 1,000,000 units of a divisible currency, the user would have chosen property type 2 and would have entered 100,000,000,000,000 for the number of properties (1 million divisible to 8 decimal places).

Once property has been created, the creator owns them at the address which broadcast the message creating them.


## Pay Dividends (Send All)

Say your company has made a huge profit and wishes to pay 1000 MSC evenly distributed among the holders of Quantum Miner shares. Doing so will use 20 bytes:

1. Transaction type = 1 for "send all" (32-bit unsigned integer, 4 bytes)
2. Currency identifier = 1 for Mastercoin (32-bit unsigned integer, 4 bytes)
3. Amount to transfer = 100,000,000,000 (1000.00000000 Mastercoins) (64-bit unsigned integer, 8 bytes, should not exceed number owned, but if it does, assume user is transferring all of them)
4. Currency ID of Payees = 6 for Quantum Miner Shares (32-bit unsigned integer, 4 bytes)

Note that this transaction is very similar to "simple send", with just one extra field. The protocol will split up the 1000 MSC among the shareholders, according to how many shares they have. 

Another use-case for this transaction type would be a giveaway, where someone wants to raise interest in their new coin or property by giving some away to everyone who owns (for instance) Mastercoins.


## Listing Something for Sale

Say you want to sell a Bible for 0.001 Mastercoins. Creating a sell offer will use a variable number of bytes due to the use of null-terminated strings:

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

1. Transaction type = 61 for Initiate purchase from listing  (32-bit unsigned integer, 4 bytes)
2. Listing ID = 0 (the ID for the listing above) (32-bit unsigned integer, 4 bytes)
3. Time limit = 259,200 seconds (3 days) (32-bit unsigned integer, 4 bytes) 
4. Price multiplier = 110% (65536*1.1 = 72090) (32-bit unsigned integer, 4 bytes) 

The reference address should point to the address which listed the Bible for sale. The seller now has 3 days to accept this buyer before the offer expires. The buyer's money is now locked in escrow until their offer expires or the purchase is complete.

The price multiplier can also be used to offer less than the suggested price. This may be viable for an established buyer and/or a stale listing.


## Accepting a Buyer

If the buyer offers a bad price, has a bad reputation, or has no reputation, then you may not wish to do business with them. If you see an offer that you like, the message to accept the offer takes X bytes:

1. Transaction type = 62 for Accept buyer offer (32-bit unsigned integer, 4 bytes)
2. Which buyer = 2 (3rd offer received) (16-bit unsigned integer, 2 bytes) 

Once a buyer has been accepted, the seller may ship the Bible.


## Leaving Feedback

Once a buyer has been accepted, they may release funds held in escrow (or destroy those funds) and leave feedback. To do so takes a variable number of bytes due to the use of a null-terminated string:

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

# Escrow-Backed User Currencies

The most important (and most controversial) feature of Mastercoins is the built-in support for users to create their own currencies out of existing Mastercoins. For the purposes of demonstrating how user currencies will work, we will use an example currency called “GoldCoins”, which are intended to track the value of one ounce of gold, and which may be stored, transferred, bought, and sold similarly to Mastercoins.

## Stability Concept

So how do we drive the value of these GoldCoins to their target value, when demand for them may surge and decline? The price of GoldCoins is decided by the balance of supply and demand. Since we can’t control the demand for GoldCoins, we must control the supply. The key to accomplishing this is to use an escrow fund which holds Mastercoins, as shown below:

![Mastercoin Protocol Layers](images/stability.png) 

The escrow fund operates like a battery on the power grid, charging when there is excess energy then discharging where there isn't enough. When there are too few GoldCoins (GoldCoin price is too high), the escrow fund releases new GoldCoins, and the escrow-battery “charges” by holding Mastercoins in escrow. When there are too many GoldCoins (GoldCoin price is too low), the escrow fund purchases some of the excess GoldCoins, thereby “discharging” the escrow-battery as it releases the stored Mastercoins.

## New Currency Creation

Say you want to create the GoldCoin currency described above, using the Gold data stream we defined. Doing so will use a varying number of bytes, due to the use of a null-terminated string. This example uses 38 bytes:
1. Transaction type = 100 for creating a new child currency (32-bit unsigned integer, 4 bytes)
2. Data Stream identifier = 3 for the Gold ticker, per our data stream example (32-bit unsigned integer, 4 bytes)
3. Escrow fund delay = 4 for 4 days (see below) (8-bit unsigned integer, 1 byte)
4. Escrow fund aggression factor = 1,000,000 for 1% (See below) (32-bit unsigned integer, 4 bytes)
5. Currency Name = “GoldCoin\0” (9 bytes)
6. Escrow Fund Initial Size = 100,000,000,000 for 1,000 Mastercoins (64-bit unsigned integer, 8 bytes, causes 1,000 Mastercoins to be debited from the currency creator and credited to the escrow fund. This number should not exceed the amount owned by the creator, but if it does, assume they are crediting all their Mastercoins to the escrow fund)
7. Escrow Fund Minimum Size = 99,000,000 for 99% (32-bit unsigned integer, 4 bytes, if the escrow fund value is ever less than 99% of all GoldCoins, the currency is dissolved and the escrow fund is distributed to GoldCoin holders who would take a 1% loss)
8. Sale/Transfer Penalty = 100,000 for 0.1% (32-bit unsigned integer, 4 bytes, any time GoldCoins are sold or transferred, 0.1% are destroyed, which improves the health of the escrow fund)


As with properties, currencies are awarded currency identifiers in the order in which they are created. Mastercoin is currency identifier 1 (bitcoin is 0), and Test Mastercoins have currency identifier 2, so if GoldCoin is the first Mastercoin-derived currency, it will get a currency identifier of 3. 

The currency held in escrow is the parent currency of the data stream. In this example it is Mastercoins, but it could also be any currency derived from Mastercoins. For instance, GoldCoins could later be held in escrow to support a currency whose data stream uses GoldCoins as a parent currency.

The escrow fund delay of 4 days means that the price of GoldCoins must be too high (or too low) for 4 days in a row before the escrow fund will take any action.

The escrow fund aggression factor determines how aggressively the escrow fund corrects the price of GoldCoins when their price diverges from their target. An escrow fund with aggression factor of 0 will never take any action. If the aggression factor is 100%, the escrow fund will take the maximum possible action (buying every GoldCoin for sale above the target price, or selling new GoldCoins to every buyer below the target price).

In the case of a 1% aggression factor, the escrow fund's first action will be to fix 1% of the error. If the error the next day is still in the same direction, the escrow fund will fix 2% of the error, then 3% the next day, and so on until it reaches 100% or the error changes direction. Once the error changes its direction, the escrow fund has done its job and it starts counting again from zero.

Items 6-8 above were added in response to the “bytemaster/d’aniel attack”, which becomes possible once malicious actors are able to short these currencies. The attack only works on currencies with underfunded escrows, and consists of a malicious actor creating a competing GoldCoin with a healthy escrow fund, which the market would presumably prefer over the GoldCoin with the unhealthy escrow fund. The malicious actor could then profit by shorting the unhealthy GoldCoin until people panicked and fled for the healthy version. More information about unhealthy escrow funds can be found in the next section.

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
* Has all output values equal & above the 'dust' threshold (currently 0.00005430 BTC)  
* An additional output is permitted for the remainder of the input (the 'change' address) 

The following conditions must also be satisfied for the transaction to be considered decode-able: 
* The reference address sequence number must be the data address sequence number + 1
* Ideally, all outputs should be the same (except the change).  In fringe cases where the change output value is equal to the other output values the change address can be identified by sequence number, which must not equal or be +/-1 of the sequence number for either the reference address or the data address 
* A last resort 'peek and decode' method may be used to identify the data packet in the event of ambiguity following the above rules.  This involves decoding each packet and looking for the correct bytes for a simple send (the majority of bytes in a Class A simple send do not change).  These byte checks are defined as:
    * Bytes two to eight must equal 00
    * Byte nine must equal 01 or 02
* Should there still be packet ambiguity or 'peek and decode' reveals more than one packet (simple sends are always one packet) the transaction is considered invalid. 

NOTE: The sequence number for a given address is defined as a 1 -byte integer stored as the first byte of each 'packet'.   Sequence numbers are continuous with 0 following 255 (256=0, 255+1=0). 

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



# Appendix B – Horrible, Awful, Very Bad Things

Speaking of insider trading, it should be clear by now that Mastercoins can be used for some very bad things. Anyone working on an implementation of the Mastercoin protocol should be very careful to warn users to not break the law of their country of residence. It is up to the user to know the laws of their country, and not (for instance) engage in sports betting in a country where sports betting is not legal.

Also, we are not securities experts, but one has to imagine that insider trading is illegal most places, if not everywhere. Also, if you think bitcoin has a reputation problem for money laundering now, just wait until you can store “USDCoins” in the block chain! At last, drug dealers and human traffickers will have the perfect currency to enable their work.

However, stable distributed currencies will be incredibly useful in a huge number of legal applications, and even modest success of this protocol could make early investors (and even those who simply hold bitcoins) very rich. Mastercoins, much like guns, are just tools, capable of being used for good or for evil. We urge our early adopters to consider how they may use Mastercoin for good, and if you get rich, how to use that money for good. It will take a lot of work to make the good outshine the evil.



# Appendix C – Disclaimer

Investing in experimental currencies is really, absurdly risky. This paper is not investment advice, and anyone predicting what will happen with experimental currencies such as those described here is indulging in the wildest sort of speculation, and that includes the speculations in the previous appendix.

Please consult your financial adviser before investing in ANY wild scheme such as this (hint: they will probably tell you to RUN and not look back unless you assure them that it is money you are totally prepared to lose).

Anyone who puts their rent money or life savings into an experiment of this type is a fool, and deserves the financial ruin they will inevitably reap from this or some other risky enterprise.
