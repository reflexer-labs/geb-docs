---
description: Chainlink integrated medianizer
---

# Chainlink Median

## 1. Summary <a id="1-introduction"></a>

The Chainlink medianizer has a similar interface to the [Governance Led Median](https://reflexer-labs.gitbook.io/geb/system-contracts/untitled-1/medianizer/governance-led) although, instead of relying on governance whitelisted oracles, it simply keeps a reference to a [Chainlink price reference contract](https://feeds.chain.link/) \(price aggregator\).

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `authorizedAccounts [usr: address]` - `addAuthorization`/`removeAuthorization`/`isAuthorized` - auth mechanisms
* `chainlinkAggregator` - address of the Chainlink price reference contract
* `medianPrice` - latest fetched price 
* `lastUpdateTime` - latest timestamp when the contract pulled a price update from Chainlink
* `multiplier` - scaling factor for the Chainlink fetched price \(e.g if the price has 8 decimals and we want it to be scaled to 18 decimals, `multiplier = 10` and `medianPrice = fetchedPrice * 10 ^ multiplier`\)
* `symbol` - the price oracle type \(ex: ETHUSD\)
* `periodSize` - the minimum delay between two consecutive updates after which the reward for updating again starts to increase
* `baseUpdateCallerReward` - starting reward for the address that calls `updateResult`
* `maxUpdateCallerReward` - max possible reward for calling `updateResult`
* `maxRewardIncreaseDelay` - max delay between two updates taken into consideration when calculating the adjusted reward
* `perSecondCallerRewardIncrease` - rate applied to `baseUpdateCallerReward` every extra second passed beyond `periodSize` seconds since the last update call
* `linkAggregatorTimestamp` - the timestamp of the Chainlink aggregator's latest price update
* `treasury` - the address of the `StabilityFeeTreasury` contract

**Functions**

* `modifyParameters` - allows governance to change the aggregator address in case Chainlink upgrades their infrastructure
* `read` - gets a non-zero price or fails
* `getResultWithValidity` - gets the price and its validity
* `updateResult` - updates the price stored in our contract by calling the aggregator

## 3. Walkthrough

Governance can update the `chainlinkAggregator` from which the contract fetches the price feed and stores it as the `medianPrice`. When reading the latest price feed, the contract also stores the timestamp when the price coming from Chainlink was posted on-chain. The system incentivizes anyone to call `updateResult` and update the median price regularly.

