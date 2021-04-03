---
description: Setter for a single collateral's debt ceiling
---

# Single Spot Debt Ceiling Setter

## 1. Summary <a id="1-introduction-summary"></a>

The `SingleSpotDebtCeilingSetter` is meant to recompute the `debtCeiling` for a single collateral type inside the `SAFEEngine`.  
  
The setter inherits functionality from the [IncreasingTreasuryReimbursement](https://docs.reflexer.finance/system-contracts/sustainability-module/increasing-treasury-reimbursement).

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `manualSetters[usr: address]` - `addManualSetter`/`removeManualSetter` - auth mechanism for addresses that can call `manualUpdateCeiling`
* `maxCollateralCeiling` - the max amount of system coins that can be generated using the collateral type with `collateralName`
* `minCollateralCeiling` - the min amount of system coins that must be generated using the collateral type with `collateralName`
* `ceilingPercentageChange` - premium on top of the current amount of debt \(backed by the collateral type with `collateralName`\) minted. This is used to calculate a new ceiling
* `lastUpdateTime` - when the debt ceiling was last updated
* `updateDelay` - enforced time gap between calls
* `lastManualUpdateTime` - last timestamp of a manual update
* `blockIncreaseWhenRevalue` - flag that blocks an increase in the debt ceiling when the redemption rate is positive
* `blockDecreaseWhenDevalue` - flag that blocks a decrease in the debt ceiling when the redemption rate is negative
* `collateralName` - the targeted collateral's name
* `safeEngine` - the `SAFEEngine` contract
* `oracleRelayer` - the `OracleRelayer` contract

**Functions**

* `modifyParameters` - modifies contract parameters
* `autoUpdateCeiling(feeReceiver: address) external` - periodically updates the debt ceiling for the collateral type with `collateralName`. Can be called by anyone
* `manualUpdateCeiling()` - authed function that allows `manualSetters` to update the debt ceiling whenever they want
* `getNextCollateralCeiling() public view returns (uint256)` - view function meant to return the new and upcoming debt ceiling. It also checks for `allowsIncrease` and `allowsDecrease`
* `getRawUpdatedCeiling() external view returns (uint256)` - view function meant to return the new and upcoming debt ceiling. It does not perform checks for `allowsIncrease` and `allowsDecrease`
* `allowsIncrease(redemptionRate: uint256`, `currentDebtCeiling: uint256`, `updatedCeiling: uint256) public view returns (allowsIncrease: bool)` - view function meant to return whether an increase in the debt ceiling is currently allowed
* `allowsDecrease(redemptionRate: uint256`, `currentDebtCeiling: uint256`, `updatedCeiling: uint256) public view returns (allowsDecrease: bool)` - view function meant to return whether a decrease in the debt ceiling is currently allowed

**Modifiers**

* `isManualSetter` - checks whether an address is part of `manualSetters`.

**Events**

* `AddManualSetter` - emitted when a new manual setter is added. Contains:
  * `account` - new manual setter address
* `RemoveManualSetter` - emitted when a manual setter address is removed/blacklisted. Contains:
  * `account` - the setter address that's removed
* `UpdateCeiling` - emitted when a new ceiling is computed and set. Contains:
  * `nextCeiling` - the new ceiling

## 3. Walkthrough <a id="2-contract-details"></a>

`autoUpdateCeiling` and `manualUpdateCeiling` can both be used to recompute the debt ceiling for the collateral type with `collateralName`. They both call `getNextCollateralCeiling` to calculate the new ceiling.

`getNextCollateralCeiling` takes into account `allowsIncrease` and `allowsDecrease` to determine whether it is allowed to computed a higher or a lower ceiling \(compared to the current one\).

Both `allowsIncrease` and `allowsDecrease` look at the current `redemptionRate` as well as at `blockIncreaseWhenRevalue` and `blockDecreaseWhenDevalue` to see whether they should allow or disallow an increase or decrease in the debt ceiling. `blockIncreaseWhenRevalue` and `blockDecreaseWhenDevalue` are by default set to zero so increases or decreases are always allowed, no matter what sign the redemption rate has.

