---
description: A set of contracts in charge with automating a GEB
---

# Automation Module

**Relevant smart contracts:**

* [CollateralAuctionThrottler](https://github.com/reflexer-labs/geb-collateral-auction-throttler/blob/master/src/CollateralAuctionThrottler.sol)
* [SingleSpotDebtCeilingSetter](https://github.com/reflexer-labs/geb-debt-ceiling-setter/blob/master/src/SingleSpotDebtCeilingSetter.sol)
* [ESMThresholdSetter](https://github.com/reflexer-labs/geb-esm-threshold-setter/blob/master/src/ESMThresholdSetter.sol)

## 1. Overview

The **Automation Module** is a set of contracts that automate parameter setting in a GEB deployment. They are meant to ease the burden of managing a system and allow the community to focus their efforts on other areas.

## 2. Component Descriptions

* `CollateralAuctionThrottler` - this contract bounds the amount of bad debt that's waiting to be covered by collateral auctions at any given time
* `SingleSpotDebtCeilingSetter` - this contract recomputes the debt ceiling for a specific collateral type by looking at the current `globalDebt` in the `SAFEEngine`
* `ESMThresholdSSetter` - this contract recomputes the threshold needed to burn and trigger settlement using the `ESM`

## 3. Risks

### Smart Contract Bugs <a id="coding-errors"></a>

* A bug in the `CollateralAuctionThrottler` could prevent the `LiquidationEngine` from liquidating any SAFE by setting `onAuctionSystemCoinLimit` to an extremely low value
* A bug in the `SingleSpotDebtCeilingSetter` could set an extremely low ceiling or it could block any further ceiling updates and thus not allow the system to issue more system coins
* A bug in the `ESMThresholdSetter` could set the `ESM` threshold to an extremely low value which would make shutdown extremely easy to execute or to a very high value which would make shutdown almost impossible to execute

### Misconfiguration

* Governance can misconfigure the throttler and make it compute extremely low values for the`onAuctionSystemCoinLimit`
* Governance can also misconfigure the `SingleSpotDebtCeilingSetter` to the point where it stops calculating new ceilings \(by setting a large delay between recalculations\) or it can calculate large or low ceilings \(compared to the current amount of debt in the system\)
* `ESMThresholdSetter` can be misconfigured and it can set extremely high or extremely low thresholds in the `ESM`

## 4. Governance Minimization

All the automation contracts are part of Level 2 governance minimization.



