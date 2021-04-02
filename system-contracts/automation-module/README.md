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
* 
