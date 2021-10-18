---
description: An incomplete list of things you can build with or on top of RAI
---

# RAI Use-Cases

### **Unique Money Markets**

If Alice pays 5% per year to borrow RAI from a money market and the RAI redemption rate is -10% per year, she is effectively earning 5% year. This is because of the expectation that RAI's market price will go down by 10% in one year. On the other hand, Bob might be lending RAI at 4% per year, but if the redemption rate is -10%, his net rate is -6%.\
\
There's the other scenario where Bob is lending RAI at 4% per year and the redemption rate is 10% per year. In total, Bob is earning 14% annually on his position (assuming that RAI will appreciate in value by 10% in the next year). Meanwhile, Alice, who's borrowing RAI at 5% per year, is paying a total of 15% (5% as the money market borrow rate plus the expected 10% appreciation in RAI's price over one year).

### Stacked Funding Rates

If an exchange or protocol decides to offer RAI perpetuals, they will essentially allow traders to stack funding rates on top of each other. \
\
The redemption rate is similar but not identical to a funding rate. The net funding rate on a RAI perpetual is a combination of the funding rate on the platform/exchange that lists the perpetual and the redemption rate.\
\
RAI is the first asset ever created that allows this.

### Options

A developer can build an options protocol which takes into account changes in the redemption rate in order to determine the price of puts/calls. This is because the redemption rate can be thought of as an intrinsic interest rate for RAI.

### Pegged Coins/Synthetic Assets

Projects building pegged coins can use RAI as a more stable alternative to ETH. In case of a severe ETH market crash, RAI can offer its holders more time to unwind their positions before they get liquidated.

### Yield Aggregator

Protocols that deploy capital in order to get the best yield for their users (e.g [Yearn](https://yearn.finance)) can leverage RAI (and its intrinsic redemption rate) to boost returns. For example, combining RAI's positive redemption rate with lending on [Compound](https://compound.finance) or [Aave](https://aave.com) is one possible way to optimize earnings.

### **Sophisticated Arbitrageurs**

Arbitrageurs (or otherwise traders in general) can look at the redemption price behavior and combine this datapoint with others (e.g market sentiment) in order to find the ideal time when they should execute a trade.

Some arbers can offer specialized RAI trading services to pools of capital. They can be allowed to flashloan funds and split the profit with the pool.

### **Portfolio Management Strategies**

Anyone can create [Set Protocol](https://www.tokensets.com) sets or [Balancer](https://balancer.finance) pools that offer diversified exposure to RAI's redemption rate as well as other yield bearing assets (e.g cTokens, aTokens).
