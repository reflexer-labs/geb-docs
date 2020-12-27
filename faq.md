---
description: Frequently asked questions about RAI and GEB
---

# FAQ

### What is RAI?

RAI is an ETH backed stable asset with a [managed float regime](https://en.wikipedia.org/wiki/Managed_float_regime). The RAIUSD exchange rate is determined by supply and demand while the protocol that issues RAI tries to stabilize its price by constantly de- or revaluing it.

The supply and demand mechanic plays out between two parties: SAFE users \(those who generate RAI with their ETH\) and RAI holders \(those who hold, speculate on or use RAI in other protocols and apps\).

Compared to protocols that try to defend a [fixed exchange rate](https://www.investopedia.com/terms/f/fixedexchangerate.asp) between their native stable asset and fiat \(DAI/USD, sUSD/USD etc\), RAI's monetary policy offers a couple of advantages:

* Flexibility: the protocol can devalue or revalue RAI in response to changes in RAI's market price. This process transfers value between SAFE users and RAI holders and incentivizes both parties to bring the market price back to a target chosen by the protocol. The mechanism is similar to countries [devaluing](https://www.investopedia.com/terms/d/devaluation.asp) or [revaluing](https://www.investopedia.com/terms/r/revaluation.asp) their currencies in order to combat a trade imbalance. The "trade imbalance" in RAI's case happens between RAI and SAFE users.
* Discretion: the protocol itself is free to change the target exchange rate to its own advantage. It can attract or repel capital whenever it wants.

At the same time, a managed float can cause uncertainty due to the fact that the price varies day by day. This uncertainty can be beneficial though: developers are incentivized to build all sorts of derivative products on top of RAI \(such as futures and options\) in order to take advantage of its [repricing mechanism](https://medium.com/reflexer-labs/stability-without-pegs-8c6a1cbc7fbd).

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

### Is RAI a stablecoin?

No. Stablecoins are pegged or oscillating around a specific value \(usually pegged to fiat coins such as USD, EUR etc\). 

RAI, on the other hand, is not pegged to anything. The system behind RAI only cares about the market price getting as close as possible to the redemption price. The redemption price will almost always float \(thus, it won't be pegged\) in order to compel system participants to bring the market price toward it.

### Why would I hold RAI when the system devalues the token?

This is exactly what the system wants you to ask yourself when it charges a negative interest rate. The system is trying to incentivize RAI holders to sell and bring the market price down and close to the redemption price.

### Isn't RAI growth bounded by ETH growth?

Short answer: yes. Nevertheless, we decided to build a pure ETH system for several reasons:

* Social scalability: we believe the most successful DeFi protocols will be the ones that act as a trust minimized operating system. You can build on top of them without the fear that the rules will drastically change and break your application. For this reason we want to progressively remove control over RAI.
* Simplicity: it is easier to explain RAI's behaviour in contrast to ETH as opposed to a basket of assets.
* Proof of concept: a system backed by a single collateral type is easier to manage than a multi-collateral one. It allows us to test our hypotheses without layering extra risk and overhead

Even if RAI is backed by a single collateral type it does **not** mean that we cannot create other systems later on which are backed by multiple/different assets.

### How do you plan to grow RAI so it achieves its maximum potential?

RAI is meant to help other projects take advantage of its redemption rate as well as provide a more stable source of borrowing power compared to ETH.  
  
We believe that growing alongside other projects and helping builders leverage RAI will make the protocol successful. In short: RAI's success depends on other projects' well being.

### Can you summarize what users should do so they can take advantage of the PID?

1. When the redemption rate is positive, SAFE users should repay their debt, RAI users should buy more RAI
2. When the redemption rate is negative, SAFE users should mint more debt, RAI users should sell RAI

### What happens if the redemption rate is positive and I just add more collateral in my SAFE instead of repaying my debt?

Two things can happen in this case:

* RAI holders/users \(who know that a higher redemption price means that they can redeem more collateral in case the system is settled\) can still long RAI and make the market price go above redemption. Once the market price goes up, it will be more expensive for you to buy RAI and repay your SAFE's debt
* Your capital efficiency will decrease because you need to add more and more collateral just to maintain your current collateralization ratio 

### What is the difference between the redemption rate and the borrow rate?

A system like RAI has two types of rates:

* The borrow rate which is an interest rate charged on open SAFEs. The borrow rate will usually be fixed or bounded
* The redemption rate: this is the rate at which RAI \(or RAI-like assets\) are devalued or revalued

### Why would I want to hold something that is not pegged and yet does not behave like ETH or BTC?

We often get this question from people who are used to holding assets such as DAI or USDC which seem more "stable" because they try to target a specific peg.

While it is true that the mechanism behind RAI may cause more uncertainty vs pegged coins because of the floating redemption price, it also comes with its own perks:

* RAI is trader friendly. A trader can, for example, analyze the market sentiment as well as look at the current redemption rate in order to decide on whether they should long or short RAI
* RAI holders \(longs\) can benefit from exposure to a positive redemption rate \(revaluation\)
* Shorts may also benefit from RAI devaluation

It is also worth considering that only some fiat currencies are pegged, while others float and they are still considered "stable". Check out [this classification](https://www.imf.org/external/np/mfd/er/2004/eng/0604.htm) of exchange rate arrangements which shows the full spectrum of stability.

### Why would I want to mint RAI?

We have both short and long term plans meant to attract borrowers and improve the experience of interacting with the protocol:

* Getting paid for opening and managing SAFEs: when RAI is devalued, SAFE users are "paid" because the value of their debt shrinks compared to the value of their collateral
* Capped borrow rate: in the long run, RAI will have a capped \(and small\) borrow rate which makes the cost of maintaining a SAFE more predictable. Governance can, in theory, set the borrow rate to 0% although this prevents the system from accruing surplus that's [used to incentivize keepers](https://docs.reflexer.finance/system-contracts/sustainability-module/stability-fee-treasury) to update core components such as oracles and the PID. A 0% borrow rate would also prevent the protocol from building a surplus buffer meant to settle bad debt that couldn't be covered by collateral auctions
* Insurance for SAFEs: in the long run we can allow SAFE users to attach a wide variety of insurance contracts meant to protect their positions against liquidation
* No exposure to assets with counterparty risk: RAI will only be backed by ETH. Borrowers are not exposed to riskier crypto assets or real world collateral
* Superior collateral factors: as we improve the efficiency of our [collateral auctions](https://docs.reflexer.finance/system-contracts/auction-module/fixed-discount-collateral-auction-house) and add insurance contracts for SAFEs, we can lower the collateral requirements for borrowing RAI

### What are RAI's use-cases?

The following is a non-exhaustive list of use-cases we envision for RAI:

* Portfolio diversification: RAI offers dampened exposure to ETH's price moves
* Source of yield: traders can earn "yield" when RAI's market price follows the redemption price 
* DeFi collateral: RAI can be used as an ETH supplement or alternative collateral in DeFi protocols due to the fact that it dampens ether's price moves and gives users more time to react to market shifts
* DAO reserve asset: DAOs can keep RAI on their balance sheet and get exposure to ETH without being affected by its full market swings

### Will RAI always return to the same initial value/peg?

RAI is not designed to be pegged to anything, so it may never return to the same value it started at. Similar to many fiat currencies \(EUR, GBP etc\), RAI will float around, being influenced by market forces \(supply & demand\) and by the incentives that the PID controller offers to SAFE users and RAI holders.

### What are the assumptions behind RAI's mechanism?

RAI's success depends on three main factors:

1. Narrative: similar to other protocols \(ranging from Multi Collateral DAI to L1's such as Bitcoin and Ethereum\), people need to believe that a system works in order for it to actually work
2. Liquidity: the more liquidity RAI has, the harder it is for malicious parties to manipulate its market price
3. The presence of arbitrageurs: similar to other stable assets, there need to be traders who arb the difference between the asset's market and redemption \(or target\) prices

It's worth noting that the narrative attracts liquidity and arbitrageurs which in turn can further strengthen the narrative.

### What can I build with RAI?

The following are a couple of examples of what's possible to build with RAI.

#### Unique money markets

If Alice pays 5% per year to borrow RAI from a money market and the RAI redemption rate is -10% per year, she is effectively earning 5% year. This is because of the expectation that RAI's market price will go down by 10% in one year. On the other hand, Bob might be lending RAI at 4% per year, but if the redemption rate is -10%, his net rate is -6%.  
  
There's the other scenario where Bob is lending RAI at 4% per year and the redemption rate is 10% per year. In total, Bob is earning 14% annually on his position \(assuming that RAI will appreciate in value by 10% in the next year\). Meanwhile, Alice, who's borrowing RAI at 5% per year, is paying a total of 15% \(5% as the money market borrow rate plus the expected 10% appreciation in RAI's price over one year\).

#### Put/call options

A developer can build an options protocol which takes into account changes in the redemption rate in order to determine the price of puts/calls. This is because the redemption rate can be thought of as an intrinsic interest rate for RAI.

#### Pegged coins

Projects building pegged coins can use RAI as a more stable alternative to ETH. In case of a severe ETH market crash, RAI can offer its holders more time to unwind their positions before they get liquidated.

#### Yield aggregator

Protocols that deploy capital in order to get the best yield for their users \(e.g [Yearn](https://yearn.finance/)\) can leverage RAI \(and its intrinsic redemption rate\) to boost returns. For example, combining RAI's positive redemption rate with lending on [Compound](https://compound.finance/) or [Aave](https://aave.com/) is one possible way to optimize earnings.

