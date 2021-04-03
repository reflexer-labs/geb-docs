---
description: A contract that recomputes the on auction system coin limit
---

# Collateral Auction Throttler

## 1. Summary <a id="1-introduction-summary"></a>

The `CollateralAuctionThrottler` is meant to recompute the `onAuctionSystemCoinLimit` for the `LiquidationEngine` according to the latest global debt supply.  
  
The throttler inherits functionality from the [IncreasingTreasuryReimbursement](https://docs.reflexer.finance/system-contracts/sustainability-module/increasing-treasury-reimbursement).

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `updateDelay` **-** minimum ****delay between updates
* `backupUpdateDelay` - delay since the last update time after which `backupLimitRecompute` can be called
* `globalDebtPercentage` - percentage of global debt taken into account in order to set `LiquidationEngine.onAuctionSystemCoinLimit`
* `minAuctionLimit` - the minimum value that can be set for `onAuctionSystemCoinLimit`
* `lastUpdateTime` - last timestamp when the `onAuctionSystemCoinLimit` was updated
* `liquidationEngine` - the `LiquidationEngine` contract
* `safeEngine` - the `SAFEEngine` contract
* `surplusHolders` - list of surplus holders taken into account when recomputing the`onAuctionSystemCoinLimit`

**Functions**

* `modifyParameters` - modify contract parameters
* `recomputeOnAuctionSystemCoinLimit(feeReceiver: address)` - recompute and set the new `onAuctionSystemCoinLimit`
* `backupRecomputeOnAuctionSystemCoinLimit()` - backup function for recomputing the `onAuctionSystemCoinLimit` in case of a severe delay since the last update



\*\*\*\*

