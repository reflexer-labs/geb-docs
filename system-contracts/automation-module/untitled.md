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

## 3. Walkthrough <a id="2-contract-details"></a>

`recomputeOnAuctionSystemCoinLimit` is the core function used to periodically recompute the `onAuctionSystemCoinLimit` and set it in the `liquidationEngine`. Anyone can call this function as long as at least `updateDelay` seconds have passed since the last update. The function rewards the caller with surplus coming from the `treasury`.

`backupRecomputeOnAuctionSystemCoinLimit` is a backup function that can be called if at least `backupUpdateDelay` seconds have passed since the `lastUpdateTime`. As the name mentions, it is meant to work as a backup in case `recomputeOnAuctionSystemCoinLimit` hasn't been called in a long time \(e.g because of bugs\).

\*\*\*\*

