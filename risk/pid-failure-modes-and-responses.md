---
description: How to react in different scenarios where the redemption rate is ineffective
---

# PID Failure Modes & Responses

## Failure Scenarios

The following is a list of known PID failure modes and possible responses or fixes for each one of them. Note that in order to minimize the risk of the PID failing, governance should activate it only after the reflex index has a minimum, mandatory liquidity level on exchanges as well as plenty of users interacting with the system. 

### Market Manipulation

Although improbable \(in case the PID is fed a TWAP feed for the reflex index market price\) after an index gets to scale, market manipulation is always a concern that can make the PID controller destabilize the system. In this scenario, governance has two options: pause the controller until there is more liquidity on exchanges or globally settle the system.

### Lack of Liquidity on Exchanges

Governance must ensure at all times that there is enough indexes on exchanges vs indexes locked in other applications. This is why governance must specify two KPIs:

1. An absolute minimum amount of liquidity that must be on the exchanges from which the PID is pulling market price data
2. A minimum percentage of indexes out of the total oustanding supply that must be at all times on exchanges vs the percentage of indexes locked/used in other applications

In case the index liquidity drops below any of the two limits specied above, governance is advised to pause the PID and restart it only after the liquidity improves.

**NOTE**: lack of liquidity will increase the risk of market manipulation \(as seen in Proto RAI\).

### Skewed Incentives

In case governance sets up a system to incentivize the growth of a reflex index, these incentives may interfere with the PID's and cause the system to become unstable. In this scenario, governance should look at the following solutions:

1. Offer less growth incentives over a longer period of time
2. Pause the controller until the growth campaign/s end
3. Make the PID weaker so it still affects the system but gives participants more time to react
4. Completely stop growth campaigns
5. Find a way to incentivize market making alongside growth

### Negative Feedback Turning Positive

There are cases when, even if there is no market manipulation, no skewed incentives and there's plenty of liquidity on exchanges, the market might not react to the redemption rate incentives and the redemption price would continue to go in a single direction for a long period of time.

In this scenario there are three possible solutions:

1. Temporarily pause the PID and wait for the market to come closer toward redemption
2. Temporarily pause the PID and build a second controller that modifies the stability fee. In this scenario the redemption rate controller would only be used when the market price is consistently above redemption and the stability fee controller would be used when the market price is below redemption. Choosing this option means that governance may need to have long term control over the [TaxCollector](https://docs.reflexer.finance/system-contracts/money-market-module/tax-collector) and there will need to be more governance over rate setting in general.
3. Trigger global settlement and allow the system to shut down using the redemption price.

## Failure Prevention

In order to make market manipulation as expensive as possible, we propose the following liquidity threshold for the RAI reflex index:

* There must be at least $1M worth of liquidity on the exchange/s that the RAI oracle is pulling a price feed from
* At least 3% of the RAI supply must be on the exchange/s from which the system is pulling a price feed from

In addition to this, the PID controller should not be set to its full force right when RAI is launched as it may destabilize the system.

