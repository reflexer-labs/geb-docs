---
description: The protocol's liquidation mechanism
---

# Liquidation Engine

## 1. Summary <a id="1-introduction-summary"></a>

The `LiquidationEngine` enables external actors to liquidate CDPs and send their collateral to the [`CollateralAuctionHouse`](https://reflexer-labs.gitbook.io/geb/system-contracts/untitled/untitled-2) as well as send a portion of their debt to the `AccountingEngine`.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `contractEnabled` - must be `1` for the `LiquidationEngine` to `liquidateCDP` s. ****Used ****to indicate whether the contract is enabled.
* `authorizedAccounts[usr: address]` - addresses allowed to call `modifyParameters()` and `disableContract()`
* `cdpEngine` - address that conforms to a `CDPEngineLike` interface. It cannot be changed after the contract is instantiated.
* `accountingEngine` - address that conforms to an `AccountingEngineLike` interface.
* `chosenCDPSaviour[collateralType: bytes32, cdp: address]` - stores the `CDPSaviour` chosen by a `cdp` user in order to save their position from liquidation.
* `cdpSaviours[saviour: address]` - stores contract addresses that can be used as `CDPSaviour`s.
  * A `CDPSaviour` can be "attached" to a `cdp` by its owner. When an external actor calls `liquidateCDP`, the `CDPSaviour` will try to add more collateral in the targeted `cdp` and thus \(potentially\) save it from liquidation.
* `mutex[collatralType: bytes32, cdp: address]` - helps with preventing re-entrancy when `liquidateCDP` calls a `CDPSaviour` in order to add more collateral to a position.
* `collateralTypes` **-** stores `CollateralType` structs

**Data Structures**

* `CollateralType`:
  * `collateralAuctionHouse` - the address of the contract that auctions a specific collateral type.
  * `liquidationPenalty` - penalty applied to a CDP when it is liquidated \(extra amount of debt that must be covered by an auction\).
  * `collateralToSell` - maximum amount of collateral to sell in one auction.

**Modifiers**

* `isAuthorized` ****- checks whether an address is part of `authorizedAddresses` \(and thus can call authed functions\).

**Functions**

* `disableContract()` - disable the liquidation engine.
* `connectCDPSaviour(saviour: address)` - governance controlled address that whitelists a `CDPSaviour`.
* `disconnectCDPSaviour(saviour: address)` - governance controlled address that blacklists a `CDPSaviour`.
* `addAuthorization(usr: address)` - add an address to `authorizedAddresses`.
* `removeAuthorization(usr: address)` - remove an address from `authorizedAddresses`.
* `modifyParameters(parameter: bytes32`, `data: address)` - modify an `address` variable.
* `modifyParameters(collateralType: bytes32`, `parameter: bytes32`, `data: uint256)` - modify a collateral specific `uint256` parameter.
* `modifyParameters(collateralType: bytes32`, `parameter: bytes32`, `data: address)` - modify a collateral specific `address` parameter.
* `liquidateCDP(collateralType: bytes32`, `cdp: address)` - will revert if `lockedCollateral` or `generatedDebt` are larger than or equal to 2^255.
* `protectCDP(bytes32, address, address)` will revert if the proposed `CDPSaviour` address was not whitelisted by governance.

#### **Events** <a id="events"></a>

* `Liquidate`- emitted when a `liquidateCDP(bytes32, address)` is successfully executed. Contains:

  * `collateralType`: Collateral
  * `safe`: SAFE address
  * `collateralAmount`: see `collateralToSell` in `liquidateSAFE`
  * `debtAmount`: see `cdpDebt` in `liquidateSAFE`
  * `amountToRaise`: see `amountToRaise` in `liquidateSAFE`
  * `collateralAuctioneer`: address of the `CollateralAuctionHouse` contract
  * `auctionId`: ID of the auction in the `CollateralAuctionHouse` 

* `SaveSAFE`- emitted when the liquidated SAFE has a `SAFESaviour` attached to it and an external actor calls `liquidateSAFE(bytes32, address)`.

  Contains:

  * `collateralType`: The collateral type of the saved SAFE
  * `safe`: SAFE address
  * `collateralAdded`: amount of collateral added in the SAFE

**Notes**

* `liquidateSAFE`will not leave a SAFE with debt and no collateral.
* `protectSAFE` will revert if the chosen `SAFESaviour` address was not whitelisted by governance.

## 3. Walkthrough

`liquidateCDP` can be called at anytime but will only succeed if the CDP is underwater. A CDP is underwater when the result of its collateral \(`lockedCollateral`\) multiplied by the collateral's liquidation price \(`liquidationPrice`\) is smaller than its present value debt \(`generatedDebt` times the collateral's `accumulatedRates`\). 

`liquidationPrice` is the oracle-reported price scaled by the collateral's liquidation ratio. There is a clear distinction between liquidation and safety ratios \(even though the two can be equal in value\):

* Safety ratios are the minimum collateralization ratios used when generating debt against a CDP's collateral. They can be more conservative \(higher\) than liquidation ratios
* Liquidation ratios are the minimum collateralization ratios under which CDPs are liquidated

