This document is collaboratively edited. Pull requests welcome.

Why Mastercoins Have Value
============

It’s worth addressing the specific use case which is the poster-child for App Coins, Mastercoin. It’s the first major project to use this model, and arguably the most well-known.

From the very beginning, the Mastercoin model hasou always been “build useful features on top of Bitcoin, and make each feature use mastercoins in some way”. For mastercoins to have value, the following two conditions must be true:

1. The Master Protocol must be used.
2. The features that make Master Protocol useful are enabled by Mastercoin.

If people need mastercoins, they will have value. The value of mastercoins is therefore expected to have a direct correlation to how useful the Master Protocol is, and also to how well each feature fulfills the promise of using mastercoins.

Following is a high level overview of each Master Protocol feature in [our spec](https://github.com/mastercoin-MSC/spec/blob/master/README.md), and how each feature uses mastercoins (some features described below are not implemented yet):

1. User-created coins/tokens and distributed exchange
..1. Can optionally use mastercoins for fundraising
..2. Every user coin trades against Mastercoin on the distributed exchange*
..3. Mastercoin is the only exit to bitcoin using the distributed exchange. This means if you have a user coin and want Bitcoin, you must either get a centralized exchange to list your coin, or use Mastercoin
..4. Properties may be promoted (get preferential placement) in Mastercoin client default listings by burning Mastercoins
2. Betting and data-feeds (uncensorable betting on anything imaginable)
..1. Mastercoin, as the “currency of the realm” is the currency for placing bets*
3. Distributed e-commerce (decentralized eBay)
..1. Mastercoin, as the “currency of the realm” is the currency for buying and selling physical goods*
..2. Certain amounts of Mastercoin are burned by the reputation/feedback system
4. Security features (savings addresses, rate limited addresses)
..1. Apply to all Mastercoin-derived currency and not just Mastercoin, but contribute to the overall utility of the ecosystem
5. Pegged currencies (coins which maintain stable values due to a protocol-managed escrow fund)
..1. Holding mastercoins in escrow to back pegged currencies will have the effect of removing certain amounts of MSC from circulation, thus increasing the competition for the remaining MSC which is in active circulation for other features.

*To the extent other currencies are allowed in these roles, there may be a small fee, burning a small amount of Mastercoin.

Why Mastercoin Will Have Value

Let’s focus mainly on how mastercoins are needed because Mastercoin is implementing features that Bitcoin just doesn't have, and to implement those features you *must* have a unit of account available.

** Properties may be promoted (get preferential placement) in Mastercoin client default listings by burning Mastercoins

This sounds like the Twister (3) proof-of-work incentive - promoted tweets - and again I see no reason why we wouldn't see clients just turn the promotions off for being spam.

** Distributed e-commerce (uncensorable e-bay)
I'd take this out; I can't see any reason why OpenBazaar won't do this better than Master coin using just direct person-to-person Bitcoin payments.

** Datastreams
It's worth pointing out that because the Mastercoin decentralized	exchange is public the exchange itself can be an honest and accurate source of pricing information.(4) Unlike Colored Coins this exchange can be automatically acted upon by Mastercoin transactions. (though equally hybrid systems can do this in an auditable way, see below)


See also
========

1. [Why Appcoins Have Value](https://github.com/DavidJohnstonCEO/TheValueofAppCoins)
2. [Mastercoin is for Burning](http://blog.mastercoin.org/2014/06/11/mastercoin-is-for-burning/)
