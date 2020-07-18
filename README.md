---
description: The what and how of the GEB framework
---

# Introduction to GEB

GEB \(Generalized Ethereum Bonds\) is a framework for deploying systems that can issue [reflex bonds](https://medium.com/@stefan__ionescu/stability-without-pegs-8c6a1cbc7fbd). Reflex bonds are assets that dampen the volatility of their underlying collateral. They are useful as more "stable" collateral for other DeFi protocols \(compared to ETH or BTC\) or as tools that give their holders more time to react to the underlying's price moves.  
  
As an example, if a reflex bond was backed by ETH and one day ETH would significantly drop in price, the bond would delay this shock and spread it over a longer period of time. The length of the "spread" depends on how the [feedback mechanism](https://reflexer-labs.gitbook.io/geb/system-contracts/feedback-mechanism-module) is set up and on how reflex bond users react to the mechanism's incentives \(or even to the mere expectation that the mechanism will change user incentives in a certain way\).  
  
This documentation is meant to explain all the components behind GEB. Before diving in the docs, we recommend reading our original [whitepaper](https://github.com/reflexer-labs/whitepapers/blob/master/rai.pdf).  
  
GEB is a modified fork of [MCD](https://github.com/makerdao/dss) that has several core differences:

* Variable names you [can actually understand](https://docs.reflexer.finance/system-overview/naming-transition)
* An autonomous [feedback mechanism](https://docs.reflexer.finance/system-contracts/feedback-mechanism-module) that changes the incentives of system participants
* The possibility to add insurance for CDPs
* Automatic adjustment of `AccountingEngine` \(former `Vow`\) parameters
* The possibility to send stability fees at once to multiple addresses
* Two prices for each `CollateralType`: one used for generating debt, the other one used exclusively when liquidating CDPs
* A stability fee treasury that can pay for oracle calls, market making or teams who onboard collateral and take care of the system
* A settlement surplus auctioneer that sells any remaining surplus after settlement is triggered
* An Oracle Network Medianizer that does not rely on any particular price feed and can be upgraded \(within certain bounds\) by governance
* A governance minimization layer that bounds human intervention over the system
