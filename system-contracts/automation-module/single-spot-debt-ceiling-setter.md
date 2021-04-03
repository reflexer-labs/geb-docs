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



