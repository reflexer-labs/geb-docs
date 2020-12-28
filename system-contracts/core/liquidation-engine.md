---
description: The protocol's liquidation mechanism
---

# Liquidation Engine

## 1. Summary <a id="1-introduction-summary"></a>

The `LiquidationEngine` enables external actors to liquidate SAFEs and send their collateral to the [`CollateralAuctionHouse`](https://reflexer-labs.gitbook.io/geb/system-contracts/untitled/untitled-2) as well as send a portion of their debt to the `AccountingEngine`.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `contractEnabled` - must be `1` for the `LiquidationEngine` to `liquidateSAFE` s. ****Used ****to indicate whether the contract is enabled.
* `authorizedAccounts[usr: address]` - addresses allowed to call `modifyParameters()` and `disableContract()`
* `safeEngine` - address that conforms to a `SAFEEngineLike` interface. It cannot be changed after the contract is instantiated.
* `accountingEngine` - address that conforms to an `AccountingEngineLike` interface.
* `chosenSAFESaviour[collateralType: bytes32, safe: address]` - stores the `SAFESaviour` chosen by a `safe` user in order to save their position from liquidation.
* `safeSaviours[saviour: address]` - stores contract addresses that can be used as `SAFESaviour`s.
  * A `SAFESaviour` can be "attached" to a `safe` by its owner. When an external actor calls `liquidateSAFE`, the `SAFESaviour` will try to add more collateral in the targeted `safe` and thus \(potentially\) save it from liquidation.
* `mutex[collatralType: bytes32, safe: address]` - helps with preventing re-entrancy when `liquidateSAFE` calls a `SAFESaviour` in order to add more collateral to a position.
* `collateralTypes` **-** stores `CollateralType` structs
* `onAuctionSystemCoinLimit` - total amount of system coins that can be requested across all collateral auctions at any time
* `currentOnAuctionSystemCoins` - amount of system coins requested across all current collateral auctions

**Data Structures**

* `CollateralType`:
  * `collateralAuctionHouse` - the address of the contract that auctions a specific collateral type.
  * `liquidationPenalty` - penalty applied to a SAFE when it is liquidated \(extra amount of debt that must be covered by an auction\).
  * `liquidationQuantity` - maximum amount of system coins to be requested in one collateral auction.

**Modifiers**

* `isAuthorized` ****- checks whether an address is part of `authorizedAddresses` \(and thus can call authed functions\).

**Functions**

* `disableContract()` - disable the liquidation engine.
* `connectSAFESaviour(saviour: address)` - governance controlled address that whitelists a `SAFESaviour`.
* `disconnectSAFESaviour(saviour: address)` - governance controlled address that blacklists a `SAFESaviour`.
* `addAuthorization(usr: address)` - add an address to `authorizedAddresses`.
* `removeAuthorization(usr: address)` - remove an address from `authorizedAddresses`.
* `modifyParameters(parameter: bytes32`, `data: address)` - modify an `address` variable.
* `modifyParameters(collateralType: bytes32`, `parameter: bytes32`, `data: uint256)` - modify a collateral specific `uint256` parameter.
* `modifyParameters(collateralType: bytes32`, `parameter: bytes32`, `data: address)` - modify a collateral specific `address` parameter.
* `liquidateSAFE(collateralType: bytes32`, `safe: address)` - will revert if `lockedCollateral` or `generatedDebt` are larger than or equal to 2^255.
* `protectSAFE(collateralType: bytes32, address, address)` will revert if the proposed `SAFESaviour` address was not whitelisted by governance
* `removeCoinsFromAuction(rad: uint256)` - signal that an amount of system coins has been covered by a collateral auction and it can now be subtracted from `currentOnAuctionSystemCoins`
* `getLimitAdjustedDebtToCover(collateralType: bytes32`, `safe: address)` - returns the amount of debt that can currently be covered by a collateral auction for a specific safe

#### **Events** <a id="events"></a>

* `AddAuthorization` - emitted when a new address becomes authorized. Contains:
  * `account` - the new authorized account
* `RemoveAuthorization` - emitted when an address is de-authorized. Contains:
  * account - the address that was de-authorized
* `ConnectSAFESaviour` - emitted when a new SAFE savior becomes available to protect SAFEs from liquidation. Contains:
  * `saviour` - the savior's address
* `DisconnectSAFESaviour` - emitted when a SAFE savior is not available anymore to protect SAFEs. Contains:
  * `saviour` - the savior's address
* `UpdateCurrentOnAuctionSystemCoins` - emitted when `currentOnAuctionSystemCoins` is updated. Contains:
  * `currentOnAuctionSystemCoins` - the current amount of system coins being requested across all active collateral auctions
* `ModifyParameters` - emitted when a parameter is updated by an authorized account
* `DisableContract` - emitted after the contract is disabled
* `Liquidate`- emitted when a `liquidateSAFE(bytes32, address)` is successfully executed. Contains:
  * `collateralType`- collateral
  * `safe`- SAFE address
  * `collateralAmount`- see `collateralToSell` in `liquidateSAFE`
  * `debtAmount`- see `safeDebt` in `liquidateSAFE`
  * `amountToRaise`- see `amountToRaise` in `liquidateSAFE`
  * `collateralAuctioneer`- address of the `CollateralAuctionHouse` contract
  * `auctionId`- ID of the auction in the `CollateralAuctionHouse` 
* `SaveSAFE`- emitted when the liquidated SAFE has a `SAFESaviour` attached to it and an external actor calls `liquidateSAFE(bytes32, address)`.

  Contains:

  * `collateralType`- The collateral type of the saved SAFE
  * `safe`- SAFE address
  * `collateralAdded`- amount of collateral added in the SAFE

* `FailedSAFESave` - emitted when a savior fails to rescue a SAFE. Contains:
  * `failReason` - the reason for failure
* `ProtectSAFE` - emitted when a SAFE user chooses a SAFE savior for their position. Contains:
  * `collateralType` - the collateral type locked in the protected SAFE
  * `safe` - the SAFE's address
  * `saviour` - the savior's address

**Notes**

* `liquidateSAFE`will not leave a SAFE with debt and no collateral
* `liquidateSAFE` will not leave a SAFE dusty
* `liquidateSAFE` will not start a new auction if `amountToRaise + currentOnAuctionSystemCoins` exceeds `onAuctionSystemCoinLimit`
* `protectSAFE` will revert if the chosen `SAFESaviour` address was not whitelisted by governance.

## 3. Walkthrough

`liquidateSAFE` can be called at any time but will only succeed if the target SAFE is underwater. A SAFE is underwater when the result of its collateral \(`lockedCollateral`\) multiplied by the collateral's liquidation price \(`liquidationPrice`\) is smaller than its present value debt \(`generatedDebt` times the collateral's `accumulatedRates`\). 

`liquidationPrice` is the oracle-reported price scaled by the collateral's liquidation ratio. There is a clear distinction between liquidation and safety ratios \(even though the two can be equal in value\):

* Safety ratios are the minimum collateralization ratios used when generating debt against a SAFE's collateral. They can be more conservative \(higher\) than liquidation ratios
* Liquidation ratios are the minimum collateralization ratios under which SAFEs are liquidated

`liquidateSAFE` may terminate early if the owner of the SAFE that's being targeted protected their position with a saviour that manages to save it from liquidation.

