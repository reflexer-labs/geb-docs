---
description: How to react in different scenarios where the PID is ineffective
---

# PID Failure Modes & Responses

The following is a list of known PID failure modes and possible responses or fixes for each one of them. Note that in order to minimize the risk of the PID failing, governance should activate it only after the reflex index has a minimum, mandatory liquidity level on exchanges as well as 

### Market Manipulation

Although improbable \(in case the PID is fed a TWAP feed for the reflex index market price\) after an index gets to scale, market manipulation is always a concern that can make the PID controller destabilize the system. In this scenario, governance has two options: pause the controller until there is more liquidity on exchanges or globally settle the system.

### Lack of Liquidity on Exchanges

Governance must ensure at all times that there is enough indexes on exchanges vs indexes locked in other applications. This is why governance must specify two KPIs:

1. An absolute minimum amount of liquidity that must be on the exchanges from which the PID is pulling market price data
2. A minimum percentage of indexes out of the total oustanding supply that must be at all times on exchanges vs the percentage of indexes locked/used in other applications

In case the index liquidity drops below any of the two limits specied above, governance is advised to pause the PID and restart it only after the liquidity improves.

**NOTE**: lack of liquidity will increase the risk of market manipulation \(as seen in Proto RAI\).

### Skewed Incentives

In case governance sets up a system to incentivize the growth of a reflex index, these incentives may interfere with the PID's and cause the system to become unstable. In this scenario governance should look at the following 

### Negative Feedback Turning Positive



