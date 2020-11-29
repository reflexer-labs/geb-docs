---
description: Frequently asked questions about RAI and GEB
---

# FAQ

### What is RAI?

RAI is an ETH backed stable asset with a [managed float regime](https://en.wikipedia.org/wiki/Managed_float_regime). The RAIUSD exchange rate is determined by supply and demand while the protocol that issues RAI tries to stabilize its price by constantly changing user incentives.

The supply and demand mechanic plays out between two parties: SAFE users \(those who generate RAI with their ETH\) and RAI holders \(those who hold, speculate on or use RAI in other protocols and apps\).

As opposed to protocols that try to defend a [fixed exchange rate](https://www.investopedia.com/terms/f/fixedexchangerate.asp) between their native stable assets and fiat \(DAI/USD, sUSD/USD etc\), RAI's managed float offers a couple of advantages:

* Flexibility: the protocol can devalue or revalue RAI in response to changes in RAI's market price. This process transfers value between SAFE users and RAI holders and incentivizes both parties to bring the market price back to a target chosen by the protocol. The mechanism is similar to countries [devaluing](https://www.investopedia.com/terms/d/devaluation.asp) or [revaluing](https://www.investopedia.com/terms/r/revaluation.asp) their currencies in order to combat a trade imbalance. The "trade imbalance" in RAI's case happens between RAI and SAFE users.
* Discretion: the protocol itself is free to change the target exchange rate to its own advantage. It can attract or repel capital whenever it wants.

At the same time, a managed float can cause uncertainty due to the fact that the price varies day by day. This uncertainty can be beneficial to the decentralized finance industry because it incentivizes developers to build all sorts of derivative products on top of RAI such as futures and options.

### How does RAI work/behave?

The long term price trajectory of RAI is determined by the capital flow in and out of ETH \(where ETH is a proxy of the economic activity and value of the Ethereum economy\). RAI tends to appreciate if ETH has a sustained surge in price and it depreciates in case ETH's price drops.

To better understand how RAI behaves, we need to analyze its monetary policy which is made out of four elements:

* Redemption price: this is the price that the protocol wants RAI to have on the secondary market \(e.g on Uniswap\). The redemption price is used by SAFE users to mint RAI against ETH and it is also used during Global Settlement in order to allow both SAFE and RAI users to redeem collateral from the system. The redemption price almost always floats and it does not target any specific peg.
* Market price: this is the price that RAI is traded at on the secondary market \(on exchanges\).
* Redemption rate: this is the rate at which RAI is being devalued or revalued. The process of devaluing/revaluing RAI consists in the redemption rate changing the redemption price.
* Global Settlement: settlement consists in shutting down the protocol and allowing both SAFE and RAI users to redeem collateral from the system. Settlement uses the redemption \(and not the market\) price to calculate how much collateral can be redeemed by each user.

Let's walk through an example of how RAI is revalued in case of ETH capital inflow \(aka people are bullish on ETH\):

* At time T1: ETH price is $500, RAI's market and redemption prices are both $5
* At time T2: ETH price surges to $1000. RAI SAFE users suddenly have more borrowing power and generate more RAI against their collateral. SAFE users sell RAI on the secondary market \(Uniswap\), causing RAI's market price to crash to $4.
* At time T3: ETH remains at $1000 and RAI's market price is still $4. The system wants the market price to get close to the redemption price. In order to eliminate the imbalance between the market/redemption prices, the system starts to revalue RAI. Revaluing consists in setting a positive redemption rate which makes the redemption price grow every second.
* At time T4: ETH remains at $1000. RAI's redemption price is now $5.1. SAFE users are starting to realize that they can now borrow less RAI per one ETH, they can redeem less ETH during Settlement \(because RAI is now more expensive\) and that it will be more expensive to close their SAFE once the market price follows the redemption price. At the same time, RAI holders are starting to realize that they can redeem more and more ETH during settlement and they can also "earn yield" by holding RAI and assuming that the market price will \(at some point\) surge toward the redemption.
* At time T5: ETH remains at $1000. RAI's redemption price is now $5.2. RAI's market price surged to $5.2 as a result of:
  * SAFE users buying RAI in order to close their positions as soon as possible instead of later on when RAI is more expensive
  * RAI holders incrementally buying more RAI in order to "earn" more yield as a result of the eventual market price surge

When RAI is devalued \(in case of ETH capital outflow\), the opposite thing happens:

* SAFE users realize that they can mint more RAI against their ETH and that they will be able to buy cheap RAI once the market price tanks
* Token holders realize that they can redeem less ETH during Settlement and, in order to earn money, they need to short RAI

### Why would I hold RAI when the system devalues the token?

This is exactly what the system wants you to ask yourself when it charges a negative interest rate. The system is trying to incentivize RAI holders to sell and bring the market price down and close to the redemption price.

### Isn't RAI growth bounded by ETH growth?

Short answer: yes. Nevertheless, we decided to build a pure ETH system for several reasons:

* Social scalability: we believe the most successful DeFi protocols will be the ones that act as a trust minimized operating system. You can build on top of them without the fear that the rules will drastically change and break your application. For this reason we want to progressively remove control over RAI.
* Simplicity: it is easier to explain RAI's behaviour in contrast to ETH as opposed to a basket of assets.
* Proof of concept: a system backed by a single collateral type is easier to manage than a multi-collateral one. It allows us to test our hypotheses without layering extra risk and overhead

Even if RAI is backed by a single collateral type it does **not** mean that we cannot create other systems later on which are backed by multiple/different assets.

### What is the difference between the redemption rate and the borrow rate?

A system like RAI has two types of rates:

* The borrow rate which is an interest rate charged on open SAFEs. The borrow rate will usually be fixed or bounded
* The redemption rate: this is the rate at which RAI \(or RAI-like assets\) are devalued or revalued

### Why would I want to hold something that is not pegged and yet does not behave like ETH or BTC?

We often get this question from people who are used to holding assets such as DAI or USDC which seem more "stable" because they try to target a specific peg.

While it is true that the mechanism behind RAI may cause more uncertainty vs pegged coins because of the floating redemption price, it also comes with its own perks:

* RAI is trader friendly. Traders can use the managed float regime to their own advantage. Someone can, for example, analyze the market sentiment as well as look at the current redemption rate in order to decide on whether they should long or short RAI
* RAI holders \(longs\) can benefit from exposure to a positive redemption rate \(revaluation\)
* Shorts may also benefit from RAI devaluation

### What are RAI's use-cases?

The following is a non-exhaustive list of use-cases we envision for RAI:

* Portfolio diversification: RAI offers dampened exposure to ETH's price moves
* Source of yield: traders can earn "yield" when RAI's market price follows the redemption price 
* DeFi collateral: RAI can be used as an ETH supplement or alternative collateral in DeFi protocols due to the fact that it dampens ether's price moves and gives users more time to react to market shifts

### What are the assumptions behind RAI's mechanism?

RAI's success depends on three main factors:

1. Narrative: similar to other protocols \(ranging from Multi Collateral DAI to L1's such as Bitcoin and Ethereum\), people need to believe that a system works in order for it to actually work
2. Liquidity: the more liquidity RAI has, the harder it is for malicious parties to manipulate its market price
3. The presence of arbitrageurs: similar to other stable assets, there need to be traders who arb the difference between the asset's market and redemption \(or target\) prices

It's worth noting that the narrative attracts liquidity and arbitrageurs which in turn can further strengthen the narrative.
