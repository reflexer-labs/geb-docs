---
description: Basic price feed delay mechanism
---

# Oracle Security Module

## 1. Summary

The `OSM` \(Oracle Security Module\) ensures that new price values propagated from the medianizers are not taken up by the system until a specified delay has passed. Values are updated using `updateResult()` and read using `read()` and `getResultWithValidity()`. Each collateral type will have its own `OSM`.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

* `addAuthorization`/`removeAuthorization` - add or remove authorized users \(via modifications to the `authorizedAccounts` mapping\)
* `stopped` - flag that disables price feed updates if non-zero
* `priceSource` - address of medianizer that the OSM will read from
* `ONE_HOUR` - 3600 seconds
* `updateDelay` - time delay between `updateResult` calls; defaults to `ONE_HOUR`
* `lastUpdateTime` - time of last update \(rounded down to nearest multiple of `updateDelay`\)
* `Feed` struct: a struct with two `uint128` members, `value` and `isValid`. Used to store price feed data
* `currentFeed` - `Feed` struct that holds the current price value
* `nextFeed` - `Feed` struct that holds the next price value
* `stop()`/`start()` - toggle whether price feed can be updated \(by changing the value of `stopped`\)
* `changePriceSource(address)` - change data source for prices \(by setting `priceSource`\)
* `changeDelay(uint16)` - change interval between price updates \(by setting `updateDelay`\)
* `restartValue()` - similar to `stop`, except it also sets `currentFeed`and `nextFeed` to a `Feed` struct with zero values
* `getResultWithValidity()` - returns the current feed value and a boolean indicating whether it is valid
* `getNextResultWithValidity()` - returns the next feed value \(i.e. the one that will become the current value upon the next `updateResult()` call\), and a boolean indicating whether it is valid
* `read()` - returns the current feed value; reverts if it was not set by some valid mechanism
* `updateResult()` - updates the current feed value and reads the next one

## 3. Walkthrough <a id="3-key-mechanisms-and-concepts"></a>

In order for the `OSM` to work properly, an external actor must regularly call `updateResult()` inb order to update the current price and read the next one. The contract stores the timestamp of the last `updateResult()` and will not allow another update until `block.timestamp` is at least `lastUpdateTime + updateDelay`. Values are read from the `priceSource`. In case of an oracle attack, governance can call `stop()`, `restartValue()`or trigger Global Settlement.

