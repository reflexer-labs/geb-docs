---
description: The glue between price feeds and the SAFE Engine
---

# Oracle Relayer

## 1. Summary <a href="#1-introduction" id="1-introduction"></a>

The `OracleRelayer` functions as an interface contract between `FSM`s and the `SAFEEngine` and only stores the current `collateralType` list as well as the current `redemptionPrice` and `redemptionRate`. The relayer will depend on governance to set each collateral's safety and liquidation ratios and might also depend on an external feedback mechanism to update the `redemptionRate` which affects the `redemptionPrice`.

## 2. Contract Variables & Functions <a href="#2-contract-details" id="2-contract-details"></a>

**Variables**

* `contractEnabled` - settlement flag (`1` or `0`).
* `authorizedAccounts[usr: address]` - addresses allowed to call `modifyParameters()` and `disableContract()`.
* `collateralTypes[collateralType: bytes32]` - mapping of each collateral type
* `cdpEngine` - address of the `CDPEngine` contract
* `redemptionRate` - the current redemption rate that reprices the system coin internally and changes user incentives
* `_redemptionPrice` - virtual variable that does not reflect the latest `redemptionPrice`
* `redemptionPriceUpdateTime` - last time when the redemption price was updated
* `redemptionRateUpperBound` - maximum value that the `redemptionRate` can have
* `redemptionRateLowerBound` - minimum value that the `redemptionRate` can have
* `RAY` - number with 27 decimals

**Data Structures**

* `CollateralType` - struct with data about each collateral type
  * `orcl` - the address of a price feed, usually an `OSM`
  * `safetyCRatio` - the collateralization ratio used to compute the `safetyPrice` of a collateral type
  * `liquidationCRatio` - the collateralization ratio used to compute the `liquidationPrice` of a collateral type

**Modifiers**

* `isAuthorized` **** - checks whether an address is part of `authorizedAddresses` (and thus can call authed functions).

**Functions**

* `modifyParameters(parameter: bytes32`, `data: uint256)` - update a `uint256` parameter.
* `modifyParameters(parameter: bytes32`, `data: uint256)` - update a collateral related parameter.
* `modifyParameters(collateralType: bytes32`, `parameter: bytes32`, `data: address)` - update an `address` parameter.
* `addAuthorization(usr: address)` - add an address to `authorizedAddresses`.
* `removeAuthorization(usr: address)` - remove an address from `authorizedAddresses`.
* `updateRedemptionPrice()` - internal function used to update the redemption price using the  `redemptionRate`
* `redemptionPrice() external view returns (uint256)` - getter function that updates and retrieves the virtual `_redemptionPrice`
* `updateCollateralPrice(collateralType: bytes32)` - update the safety and liquidation prices of a collateral price and store them in the `CDPEngine`
* `disableContract()` - disables the relayer
* `safetyCRatio(collateralType: bytes32) external view returns (uint256)` - getter for a collateral's safety CRatio
* `liquidationCRatio(collateralType: bytes32) external view returns (uint256)` - getter for a collateral's liquidation CRatio
* `orcl(collateralType: bytes32) external view returns (address)` - getter for a collateral type's oracle

**Events**

* `AddAuthorization` - emitted when a new address becomes authorized. Contains:
  * `account` - the new authorized account
* `RemoveAuthorization` - emitted when an address is de-authorized. Contains:
  * `account` - the address that was de-authorized
* `DisableContract` - emitted when the contract is disabled.
* `ModifyParameters` - emitted when a parameter is updated.
* `UpdateRedemptionPrice` - emitted when the redemption price is updated. Contains:
  * `redemptionPrice` - the latest redemption price
* `UpdateCollateralPrice` - emitted when the safety and liquidation prices of a specific collateral price are updated. Contains:
  * `collateralType` - the collateral type whose prices are updated
  * `priceFeedValue` - the new price feed coming from the collateral's oracle
  * `safetyPrice` - the price computed by dividing the feed value by the `redemptionPrice` and then dividing the result again by the collateral's `safetyCRatio`
  * `liquidationPrice` - the price computed by dividing the feed value by the `redemptionPrice` and then dividing the result again by the collateral's `liquidationCRatio`

## 3. Walkthrough <a href="#3-key-mechanisms-and-concepts" id="3-key-mechanisms-and-concepts"></a>

### UpdateCollateralPrice <a href="#poke" id="poke"></a>

`updateCollateralPrice` is a non-authenticated function. The function takes in a `bytes32` representing a `collateralType` whose (safety and liquidation) prices need to be updated. `updateCollateralPrice` has three stages:

1. `getResultWithValidity` - interacts with the `collateralType`'s `orcl` and returns a `value` and whether it  `isValid` (a boolean which is false if the price is invalid). The second external call only happens if `isValid == true`.
2. When calculating the `safetyPrice` and the `liquidationPrice`, the `_redemptionPrice` is crucial as it defines the relationship between the system coin and one unit of collateral. The `value` from the `OSM` is  divided by the (updated) `redemptionPrice` (to get a ratio of collateral `value` to system coins) and then the result is divided again by the `collateralType.safetyCRatio` (when calculating the `safetyPrice`) and by the `collateralType.liquidationCRatio` (when calculating the `liquidationPrice`).
3. `cdpEngine.modifyParameters` is then called to update the collateral's prices inside the system.

### Redemption Price

Every time someone wants to read the `_redemptionPrice` its value will first be updated using the `redemptionRate` and then the output will be returned. We chose this design in order to ensure a smooth  `redemptionPrice` pro-ration (using the virtual variable + a state modifying getter).

### Updating the Redemption Rate

Every time the `redemptionRate` is updated, the contract makes sure to bound the value that is can be set to.

## 4. Gotchas <a id="4-gotchas"></a>

The methods in the `oracleRelayer` are relatively basic compared to most other portions of `geb`. There is not much room for user error in the single unauthed method `updateCollateralPrice`. If an incorrect `bytes32` is supplied the call will fail.

Any module that is authed against the `oracleRelayer` has full root access, and can, therefore, add and remove which `collateralTypes` can be "updateCollateralPrice"'d. While not completely breaking the system, this could cause considerable risk. An authed caller can also update the `redemptionRate` and `redemptionPrice`, causing considerable impact depending on the values used.

## 5. Failure Modes <a id="5-failure-modes"></a>

#### Coding Error

A bug in `oracleRelayer` would most likely result in the prices for collaterals and the redemptionRate not being updated anymore. In this case, the system would need to authorize a new `oracleRelayer` which would then be able to update the prices/rate. Overall this is not a catastrophic failure as this would only pause all price fluctuation for some period.

#### Feeds

The `oracleRelayer` relies upon a set of trusted oracles to provide price data. Should these price feeds fail, it would become possible for unbacked Coin to be minted, or Safes could be unfairly liquidated.

#### Spot Price Becoming Stale

When `updateCollateralPrice` is not called frequently enough, the `SAFE`'s `safetyPrice` price will become stale. This could arise for a few reasons including tragedy of the commons or miner collusion and could lead to negative outcomes such as inappropriate liquidations, or the prevention of liquidations that should be possible.