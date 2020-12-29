---
description: Winding down system operations
---

# Shutdown Module

**Relevant smart contracts:**

* [GlobalSettlement](https://github.com/reflexer-labs/geb/blob/master/src/GlobalSettlement.sol)
* [ESM](https://github.com/reflexer-labs/esm/blob/master/src/ESM.sol)

## 1. Overview

The **Shutdown Module** is in charge with winding down the system and returning all collateral back to users in case of a serious threat, such as long-term market irrationality, a hack, or a security breach.

Settlement can be triggered by the `ESM` \(Emergency Shutdown Module\) where governance needs to deposit \(and burn\) a specific amount of protocol tokens **OR** if governance has direct access to `GlobalSettlement`, they can bypass the `ESM` and settle the system.

## 2. Component Descriptions

* `GlobalSettlement` - this contract shuts down GEB and ensures that both SAFE and system coin users receive the net value of assets they are entitled to. The value of collateral that coin holders can redeem will vary depending on the system surplus or deficit at the time of settlement. It is possible that coin holders will receive less or more than [`OracleRelayer.redemptionPrice`](https://docs.reflexer.finance/system-contracts/oracle-module/oracle-relayer) worth of collateral for one coin.
* `ESM` - this contract can trigger global settlement if enough tokens are deposited \(and subsequently burned\) in it.

## 3. Risks

### Misconfigurations <a id="authorization-misconfigurations"></a>

The `ESM` may be unable to trigger shutdown \(even if sufficient protocol tokens have been committed to the contract\) if `GlobalSettlement` did not authorize it.

### Settlement Edge Cases

#### Oracle Attack <a id="critical-failure-modes"></a>

Since `GlobalSettlement` reads collateral prices from `FSM`s it is susceptible to reading bad data in case the oracles get attacked. Governance is not advised to use settlement as a solution to oracle attacks because the prices get added in the system too quickly in order for shutdown to help.

#### Other Failure Modes <a id="critical-failure-modes"></a>

* If `GlobalSettlement.shutdownCooldown` is set too high, it can result in the shutdown not being able to proceed.
* If `GlobalSettlement.shutdownCooldown` is too low, it can result in `setOutstandingCoinSupply` being called before all auctions have finished, resulting in debt being calculated incorrectly and setting wrong collateral prices.
* When `GlobalSettlement.shutdownSystem` is called all system coin holders may be left holding an unstable asset. This could result in a market price crash across all collateral types due to liquidations and sell-offs.
* `SAFEEngine` / `AccountingEngine` / `LiquidationEngine` / `OracleRelayer` / `StabilityFeeTreasury` - if they are set to malicious contracts \(inside `GlobalSettlement`\) they may cause shutdown to fail.

## 4. Governance Minimization

`GlobalSettlement` is part of Level 2 Gov Minimization. `ESM` is part of Level 1 Gov Minimization.

{% hint style="info" %}
**The** `ESMThresholdSetter` 

Before removing control from the `ESM`, governance should deploy a contract called `ESMThresholdSetter` that automatically sets the `ESM.triggerThreshold` according to the current outstanding supply of protocol tokens.
{% endhint %}

