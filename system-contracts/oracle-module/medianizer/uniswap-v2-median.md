---
description: A medianizer using the Uniswap V2 TWAP oracle implementation
---

# Uniswap V2 Median

## 1. Summary <a href="#1-introduction" id="1-introduction"></a>

The `UniswapConsecutiveSlotsPriceFeedMedianizer` is integrated with Uniswap V2 in order to provide a price feed for one of the tokens in a pool. It is also connected to a `converterFeed` in order to get the fiat value of the price quoted by the pool.

## 2. Contract Variables & Functions <a href="#2-contract-details" id="2-contract-details"></a>

**Variables**

* `defaultAmountIn` - default amount of `targetToken` used when calculating the `denominationToken` output
* `targetToken` - the token which the contract calculates the `medianPrice` for
* `denominationToken` - pair token for `targetToken`
* `uniswapPair` - the address of the Uniswap V2 pair
* `uniswapFactory` - the address of the Uniswap V2 factory
* `uniswapObservations[observation: UniswapObservation]` - array of prices taken from the Uniswap pool and the timestamps associated with each of them
* `converterPriceCumulative` - the snapshot of the latest sum of prices fetched from the `converterFeed` since the contract has been deployed
* `converterFeed` - address of the contract that offers fiat price feeds for the `denominationToken`
* `converterFeedObservations[observation: ConverterFeedObservation]` - array of fiat price feeds for the `denominationToken`
* `symbol` - the symbol of the price feed offered by the contract
* `granularity` - how many price observations are stored for the `windowSize.`As granularity increases from 1, more frequent updates are needed, but moving averages become more precise.
* `lastUpdateTime` - when the price feed was last updated
* `updates` - total number of updates up until now
* `windowSize` - the desired amount of time over which the moving average should be computed, e.g. 24 hours
* `maxWindowSize` - max window over which the moving average can be computed
* `periodSize` - this is redundant with `granularity` and `windowSize`, but stored for gas savings & informational purposes. It is the minimum amount of time that must pass between two updates.
* `converterFeedScalingFactor` - this is the denominator that correctly scales the `medianPrice` according to how many decimals the `denominationToken` has
* `medianPrice` - the latest `targetToken` median price
* `validityFlag` - flag that indicates whether the median is valid or not
* `relayer` - address of the contract that rewards others for updating the median

**Modifiers**

* `isAuthorized` **** - checks whether an address is part of `authorizedAddresses` (and thus can call authed functions).

**Functions**

* `modifyParameters` - allows governance to modify parameters.
* `timeElapsedSinceFirstObservation() public view returns (uint256)` - returns the time passed since the first observation in the window.
* `earliestObservationIndex() public view returns (uint256)` - returns the index of the earliest observation in the window.
* `getObservationListLength() public view returns (uint256, uint256)` - get the observation list length
* `uniswapComputeAmountOut(priceCumulativeStart: uint256`, `priceCumulativeEnd: uint256`, `timeElapsed: uint256`, `amountIn: uint256) public pure returns (amountOut: uint256)` - given the Uniswap V2 cumulative prices of the start and end of a period, and the length of the period, compute the average price in terms of how much amount out is received for the amount in.
* `converterComputeAmountOut(amountIn: uint256) public view returns (amountOut: uint256)` - calculate the price of an amount of tokens using the converter price feed. Used internally after the contract determines the amount of denomination tokens for `defaultAmountIn` target tokens.
* `updateResult(feeReceiver: address)` - update the `medianPrice` and pay the `feeReceiver` afterwards.
* `read` - gets a non-zero price or fails
* `getResultWithValidity` - gets the price and its validity

**Events**

* `AddAuthorization` - emitted when a new address becomes authorized. Contains:
  * `account` - the new authorized account
* `RemoveAuthorization` - emitted when an address is de-authorized. Contains:
  * `account` - the address that was de-authorized
* `ModifyParameters` **** - emitted when a parameter is updated
* `UpdateResult` - emitted when `updateResult` is called. Contains:
  * `medianPrice` - the latest median price for `targetToken`
  * `lastUpdateTime` - the current timestamp
* `FailedConverterFeedUpdate` - emitted when the contract fails to pull a price from the `converterFeed`. Contains:
  * `reason` - the failure reason
* `FailedUniswapPairSync` - emitted when the contract fails to sync the Uniswap V2 pool that it's connected to. Contains:
  * `reason` - the reason why the sync failed

## 3. Walkthrough

`updateResult` first tries to update the `converterFeed` and the Uniswap pool before it stores new observations and computes the latest median.

`read` will only return a result if the median is non-null, if `updateResult` has been successfully called at least `granularity` times, if the `validityFlag` is `1` and if  `timeElapsedSinceFirstObservation() <= maxWindowSize`. `getResultWithValidity` will return the median price and its validity (determined using the same checks as `read`).

## 4. Gotchas

This oracle depends on a pinger call within a defined frequency. THe calls should be properly incentivised.

## 5. Failure Modes (Bounds on Operating Conditions & External Risk Factors)

Uniswap V2 is vulnerable to oracle manipulation attacks. A motivated attacker could push the price in the wrong direction and update the oracle. For TWAPs that is less of an issue (it would require multiple successful calls from an attacker).