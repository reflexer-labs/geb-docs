---
description: Basic price feed delay mechanism
---

# Oracle Security Module

## 1. Summary

The `OSM` (Oracle Security Module) ensures that new price values propagated from medianizers are not taken up by the system until a specified delay has passed. Values are updated using `updateResult` and read using `read` and `getResultWithValidity`. Each collateral type will have its own unique `OSM`. Collateral type is defined as a specific asset type (whereas `ETH-A` and `ETH-B` refer to the same asset and thus have the same `OSM`).

## 2. Contract Variables & Functions <a href="#2-contract-details" id="2-contract-details"></a>

**Variables**

* `authorizedAccounts[usr: address]` - addresses allowed to call authed functions.
* `stopped` - flag that disables price feed updates if non-zero
* `priceSource` - address of medianizer that the OSM will read from
* `ONE_HOUR` - 3600 seconds
* `updateDelay` - time delay between `updateResult` calls; defaults to `ONE_HOUR`
* `lastUpdateTime` - time of last update (rounded down to nearest multiple of `updateDelay`)
* `currentFeed` - `Feed` struct that holds the current price value
* `nextFeed` - `Feed` struct that holds the next price value

**Modifiers**

* `isAuthorized` **** - checks whether an address is part of `authorizedAddresses` (and thus can call authed functions).

**Functions**

* `addAuthorization`/`removeAuthorization` - add or remove authorized users (via modifications to the `authorizedAccounts` mapping)
* `stop()`/`start()` - toggle whether the OSM price feed can be updated (by changing the value of `stopped`)
* `changePriceSource(address)` - change data source for prices (by setting `priceSource`)
* `changeDelay(uint16)` - change interval between price updates (by setting `updateDelay`)
* `restartValue()` - similar to `stop`, except it also sets `currentFeed`and `nextFeed` to a `Feed` struct with zero values
* `getResultWithValidity()` - returns the current feed value and a boolean indicating whether it is valid
* `getNextResultWithValidity()` - returns the next feed value (i.e. the one that will become the current value upon the next `updateResult()` call), and a boolean indicating whether it is valid
* `read()` - returns the current feed value; reverts if it was not set by some valid mechanism
* `updateResult()` - updates the current feed value and reads the next one

**Data Structures**

* `Feed` - struct used to store price feed data. Contains:
  * `value` - the feed value
  * `isValid` - whethet the feed value is valid

## 3. Walkthrough <a href="#3-key-mechanisms-and-concepts" id="3-key-mechanisms-and-concepts"></a>

In order for the `OSM` to work properly, an external actor must regularly call `updateResult()` to update the current price and read the next one. The contract stores the timestamp of the last `updateResult()` and will not allow another update until `block.timestamp` is at least `lastUpdateTime + updateDelay`. Values are read from the `priceSource`. In case of an oracle attack, governance can call `stop()` or`restartValue()`

## 4. OSM Variations

#### SelfFundedOSM

This contract pulls funds from the [StabilityFeeTreasury](https://github.com/reflexer-labs/geb/blob/master/src/single/StabilityFeeTreasury.sol) so it can reward addresses for calling`updateResult`.&#x20;

**ExternallyFundedOSM**

This contract calls an [FSMWrapper](https://github.com/reflexer-labs/geb-fsm/blob/master/src/FSMWrapper.sol) in order to reward addresses that call `updateResult`.

&#x20;

## 5. Gotchas \(Potential Sources of User Error\)

N/A

## 6. Failure Modes \(Bounds on Operating Conditions & External Risk Factors\)

#### `updateCollateralPrice()` is not called promptly, allowing malicious prices to be swiftly uptaken

For several reasons, `updateCollateralPrice()` is always callable as soon as `block.timestamp / updateDelay` increments, regardless of when the last `updateCollateralPrice()` call occurred \(because `lastUpdateTime` is rounded down to the nearest multiple of `updateDelay`\). This means the contract does not actually guarantee that a time interval of at least `updateDelay` seconds has passed since the last `updateCollateralPrice()` call before the next one; rather this is only \(approximately\) guaranteed if the last `updateCollateralPrice()` call occurred shortly after the previous increase of `block.timestamp / updateDelay`. Thus, a malicious price value can be acknowledged by the system in a time potentially much less than `updateDelay`.

**This was a deliberate design decision. The arguments that favoured it, roughly speaking, are:**

* Providing a predictable time at which Prot holders should check for evidence of oracle attacks \(in practice, `updateDelay` is 1 hour, so checks must be performed at the top of the hour\)
* Allowing all OSMs to be reliably updated at the same time in a single transaction

The fact that `updateCollateralPrice` is public, and thus callable by anyone, helps mitigate concerns, though it does not eliminate them. For example, network congestion could prevent anyone from successfully calling `updateCollateralPrice()` for a period of time. If a Prot holder observes that `updateCollateralPrice` has not been promptly called, **the actions they can take include:**

1. Call `updateCollateralPrice()` themselves and decide if the next value is malicious or not
2. Call `stop()` or `restartValue()` \(the former if only `nextFeed` is malicious; the latter if the malicious value is already in `currentFeed`\)
3. Trigger emergency shutdown \(if the integrity of the overall system has already been compromised or if it is believed the rogue oracle\(s\) cannot be fixed in a reasonable length of time\)

In the future, the contract's logic may be tweaked to further mitigate this \(e.g. by **only** allowing `updateCollateralPrice()` calls in a short time window each `updateDelay` period\).

### Authorization Attacks and Misconfigurations

Various damaging actions can be taken by authorized individuals or contracts, either maliciously or accidentally:

* Revoking access of core contracts to the methods that read values, causing mayhem as prices fail to update
* Completely revoking all access to the contract
* Changing `src` to either a malicious contract or to something that lacks a `getResultWithValidity()` interface, causing transactions that `updateCollateralPrice()` the affected OSM to revert
* Calling disruptive functions like `stop` and `restartValue` inappropriately

The only solution to these issues is diligence and care regarding the `authorizedAccounts` of the OSM.

