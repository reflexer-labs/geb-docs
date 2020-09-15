---
description: An integration with the Uniswap V2 TWAP oracle
---

# Uniswap V2 Median

## 1. Summary <a id="1-introduction"></a>

The `UniswapPriceFeedMedianizer` is integrated with a Uniswap V2 in order to provide a price feed for one of the tokens in a pool. It is also connected to a `converterFeed` in order to get the fiat value of the price quoted by the pool.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `defaultAmountIn` - default amount of `targetToken` used when calculating the `denominationToken` output
* `targetToken` - the token which the contract calculates the `medianPrice` for
* `denominationToken` - pair token for `targetToken`
* `uniswapPair` - the address of the Uniswap V2 pair
* `uniswapFactory` - the address of the Uniswap V2 factory
* `uniswapObservations[observation: UniswapObservation]` -
* `converterPriceCumulative` - the snapshot of the latest sum of prices fetched from the `converterFeed` since the contract has been deployed
* `converterFeed` - address of the contract that offers fiat price feeds for the `denominationToken`
* `converterFeedObservations[observation: ConverterFeedObservation]` - array of price feeds for the `denominationToken`
* `symbol` - the symbol of the price feed offered by the contract
* `granularity` - how many price observations are stored for the `windowSize.`As granularity increases from 1, more frequent updates are needed, but moving averages become more precise.
* `lastUpdateTime` - when the price feed was last updated
* `updates` - total number of updates up until now
* `windowSize` - the desired amount of time over which the moving average should be computed, e.g. 24 hours
* `periodSize` - this is redundant with `granularity` and `windowSize`, but stored for gas savings & informational purposes. It is the minimum amount of time that must pass between two updates.
* `converterFeedScalingFactor` - this is the denominator that correctly scales the `medianPrice` according to how many decimals the `denominationToken` has
* `medianPrice` - the latest `targetToken` median price 
* `baseUpdateCallerReward` -
* `maxUpdateCallerReward` -
* `maxRewardIncreaseDelay` -
* `perSecondCallerRewardIncrease` -
* `treasury` -

