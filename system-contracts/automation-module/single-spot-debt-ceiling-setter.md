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
* maxCollateralCeiling -
* minCollateralCeiling -
* ceilingPercentageChange -
* lastUpdateTime -
* updateDelay -
* lastManualUpdateTime -
* blockIncreaseWhenRevalue -
* blockDecreaseWhenDevalue -
* collateralName -
* safeEngine -
* 
