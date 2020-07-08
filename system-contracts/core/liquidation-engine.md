---
description: The protocol's liquidation mechanism
---

# Liquidation Engine

## 1. Summary <a id="1-introduction-summary"></a>

The `LiquidationEngine` enables external actors to liquidate CDPs and send their collateral to the [`CollateralAuctionHouse`](https://reflexer-labs.gitbook.io/geb/system-contracts/untitled/untitled-2) as well as send a portion of their debt to the `AccountingEngine`.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

* `liquidateCDP(bytes32, address)` - will revert if `lockedCollateral` or `generatedDebt` are larger than or equal to 2^255.
* `liquidateCDP`will not leave a CDP with debt and no collateral.
* `protectCDP(bytes32, address, address)` will revert if the proposed `CDPSaviour` address was not whitelisted by governance.
* `authorizedAccounts` - addresses allowed to call `modifyParameters()` and `disableContract()`
* `collateralTypes` stores `CollateralType` structs
  * `CollateralType` is the struct with the address of the collateral auction contract \(`CollateralAuctionHouse`\), the penalty for that collateral to be liquidated \(`liquidationPenalty`\) and the maximum size of collateral that can be auctioned at once \(`collateralToSell`\).
* `cdpSaviours` stores contract addresses that can be used as `CDPSaviour`s
  * A `CDPSaviour` can be "attached" to a `cdp` by its owner. When an external actor calls `liquidateCDP`, the `CDPSaviour` will try to add more collateral in the targeted `cdp` and thus \(potentially\) save it from liquidation.
* `chosenCDPSaviour` stores the `CDPSaviour` chosen by a `cdp` user in order to save their position from liquidation.
* `mutex` helps with preventing re-entrancy when `liquidateCDP` calls a `CDPSaviour` in order to add more collateral to a position.
* `contractEnabled` - must be `1` for the `LiquidationEngine` to `liquidateCDP` s.
* `cdpEngine` - address that conforms to a `CDPEngineLike` interface. It cannot be changed after the contract is instantiated.
* `accountingEngine` - address that conforms to a `AccountingEngineLike` interface.

#### **Events** <a id="events"></a>

* `Liquidate`: emitted when a `liquidateCDP(bytes32, address)` is successfully executed. Contains:

  * `collateralType`: Collateral
  * `cdp`: CDP address
  * `collateralAmount`: see `collateralToSell` in `liquidateCDP`
  * `debtAmount`: see `cdpDebt` in `liquidateCDP`
  * `amountToRaise`: see `amountToRaise` in `liquidateCDP`
  * `collateralAuctioneer`: address of the `CollateralAuctionHouse` contract
  * `auctionId`: ID of the auction in the `CollateralAuctionHouse` 

* `SaveCDP`: emitted when the liquidated CDP has a `CDPSaviour` attached to it and an external actor calls `liquidateCDP(bytes32, address)`.

  Contains:

  * `collateralType`: Collateral
  * `cdp`: CDP address
  * `collateralAdded`: amount of collateral added in the CDP

## 3. Walkthrough

`liquidateCDP` can be called at anytime but will only succeed if the CDP is underwater. A CDP is underwater when the result of its collateral \(`lockedCollateral`\) multiplied by the collateral's liquidation price \(`liquidationPrice`\) is smaller than its present value debt \(`generatedDebt` times the collateral's `accumulatedRates`\). 

`liquidationPrice` is the oracle-reported price scaled by the collateral's liquidation ratio. There is a clear distinction between liquidation and safety ratios \(even though the two can be equal in value\):

* Safety ratios are the minimum collateralization ratios used when generating debt against a CDP's collateral. They can be more conservative \(higher\) than liquidation ratios
* Liquidation ratios are the minimum collateralization ratios under which CDPs are liquidated

