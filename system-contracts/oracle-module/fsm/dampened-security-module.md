---
description: >-
  An OSM-like contract that bounds price feed changes between consecutive
  updates
---

# Dampened Security Module

## 1. Summary

The `DSM` \(Dampened Security Module\) is an `OSM`-like contract that, apart from emposing a delay between prices coming from a medianizer and them being added into the system, it bounds the maximum price change between the `currentFeed` and the `nextFeed`. This ensures that in case of an oracle attack or extreme market volatility, governance has more time to react and defend the system.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `authorizedAccounts[usr: address]` - addresses allowed to call authed functions.
* `stopped` - flag that disables price feed updates if non-zero
* `priceSource` - address of the medianizer that the OSM will read from
* `ONE_HOUR` - 3600 seconds
* `updateDelay` - time delay between `updateResult` calls; defaults to `ONE_HOUR`
* `lastUpdateTime` - time of last update \(rounded down to nearest multiple of `updateDelay`\)
* `newPriceDeviation` - max deviation accepted between the current and the next feed
* `currentFeed` - `Feed` struct that holds the current price value
* `nextFeed` - `Feed` struct that holds the next price value

**Modifiers**

* `isAuthorized` ****- checks whether an address is part of `authorizedAddresses` \(and thus can call authed functions\).

**Functions**

* `addAuthorization`/`removeAuthorization` - add or remove authorized users \(via modifications to the `authorizedAccounts` mapping\)
* `stop`/`start` - toggle whether the OSM price feed can be updated \(by changing the value of `stopped`\)
* `changePriceSource(source: address)` - change data source for prices \(by setting `priceSource`\)
* `changeDelay(delay: uint16)` - change interval between price updates \(by setting `updateDelay`\)
* `changeNextPriceDeviation(deviation: uint256)` - change the `newPriceDeviation`
* `restartValue` - similar to `stop`, except it also sets `currentFeed`and `nextFeed` to a `Feed` struct with zero values
* `getResultWithValidity` - returns the current feed value and a boolean indicating whether it is valid
* `getNextResultWithValidity` - returns the next feed value \(i.e. the one that will become the current value upon the next `updateResult` call\), and a boolean indicating whether it is valid
* `getNextBoundedPrice` - get the next `currentFeed` price according to the current `newPriceDeviation`
* `getNextPriceLowerBound` - get the lower bound of the next `currentFeed` update
* `getNextPriceUpperBound` - get the upper bound of the next `currentFeed` update
* `read` - returns the current feed value; reverts if it was not set by some valid mechanism
* `updateResult` - updates the current feed value and reads the next one

**Data Structures**

* `Feed` - struct used to store price feed data. Contains:
  * `value` - the feed value
  * `isValid` - whether the price feed value is valid

