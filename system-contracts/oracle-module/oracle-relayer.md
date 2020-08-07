---
description: The glue between price feeds and the CDP Engine
---

# Oracle Relayer

## 1. Summary <a id="1-introduction"></a>

The `OracleRelayer` functions as an interface contract between `OSM`s and the `CDPEngine` and only stores the current `collateralType` list as well as the current `redemptionPrice` and `redemptionRate`. The relayer will depend on governance to set each collateral's safety and liquidation ratios and might also depend on an external [feedback mechanism](https://reflexer-labs.gitbook.io/geb/system-contracts/feedback-mechanism-module) to update the `redemptionRate` which affects the `redemptionPrice`.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `contractEnabled` - settlement flag \(`1` or `0`\).
* `authorizedAccounts[usr: address]` - addresses allowed to call `modifyParameters()` and `disableContract()`.
* `collateralTypes[collateralType: bytes32]` - mapping of each collateral type
* `cdpEngine` - address of the `CDPEngine` contract
* `redemptionRate` - the current redemption rate that reprices the system coin internally and changes user incentives
* `_redemptionPrice` - virtual variable that does not reflect the latest `redemptionPrice`
* `redemptionPriceUpdateTime` - last time when the redemption price was updated



* `CollateralType` - struct with data about each collateral type
  * `orcl` - the address of a price feed, usually an `OSM`
  * `safetyCRatio` - the collateralization ratio used to compute the `safetyPrice` of a collateral type
  * `liquidationCRatio` - the collateralization ratio used to compute the `liquidationPrice` of a collateral type
* `modifyParameters` - function used to change the relayer's parameters
* `updateRedemptionPrice` - internal function used to update the redemption price using the \(per-second\) `redemptionRate`
* `redemptionPrice` - getter function that updates and retrieves the virtual `_redemptionPrice`
* `updateCollateralPrice(bytes32: collateralType)` - update the safety and liquidation prices of a collateral price and store them in the `CDPEngine`
* `disableContract` - disables the relayer
* `safetyCRatio(collateralType: bytes32)` - getter for a collateral's safety CRatio
* `liquidationCRatio(collateralType: bytes32)` - getter for a collateral's liquidation CRatio
* `orcl(bytes32: collateralType)` - getter for a collateral type's oracle

**Events**

* `UpdateCollateralPrice`: emitted when `updateCollateralPrice(bytes32)` is successfully executed.  Contains:
  * `collateralType` - the collateral who's safety and liquidation prices have just been updated
  * `priceFeedValue` - the latest collateral price feed
  * `safetyPrice` - the price computed by dividing the feed value by the `redemptionPrice` and then dividing the result again by the collateral's `safetyCRatio`
  * `liquidationPrice` - the price computed by dividing the feed value by the `redemptionPrice` and then dividing the result again by the collateral's `liquidationCRatio`

## 3. Walkthrough <a id="3-key-mechanisms-and-concepts"></a>

### UpdateCollateralPrice <a id="poke"></a>

`updateCollateralPrice` is a non-authenticated function. The function takes in a `bytes32` representing a `collateralType` whose prices need to be updated. `updateCollateralPrice` has three stages:

1. `getResultWithValidity` - interacts with the `collateralType`'s `OSM` and takes back a `value` and whether it  `isValid` \(a boolean which is false if there was an error in the `OSM`\). The second external call only happens if `isValid == true`.
2. When calculating the `safetyPrice` and the `liquidationPrice`, the `_redemptionPrice` is crucial as it defines the relationship between the system coin and one unit of collateral. The `value` from the `OSM` is  divided by the \(updated\) `redemptionPrice` \(to get a ratio of collateral `value` to system coins\) and then the result is divided again by the `collateralType.safetyCRatio` \(when calculating the `safetyPrice`\) and by the `collateralType.liquidationCRatio` \(when calculating the `liquidationPrice`\).
3. `cdpEngine.modifyParameters` is then called to update the collateral's prices inside the system.

### RedemptionPrice

Every time someone wants to read the `redemptionPrice` its value will first be updated using the stored `redemptionRate` and then the output will be returned. We chose this design in order to ensure a smooth `redemptionPrice` pro-ration \(using the virtual variable + a state modifying getter\).

