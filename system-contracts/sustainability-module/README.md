---
description: The protocol's resource fund
---

# Sustainability Module

**Relevant smart contracts:**

* [StabilityFeeTreasury](https://github.com/reflexer-labs/geb/blob/master/src/StabilityFeeTreasury.sol)

## 1. Overview

The **Sustainability Module** provides funds to critical system components such as the [Oracle Network Medianizer](https://reflexer-labs.gitbook.io/geb/system-contracts/oracle-module/medianizer/oracle-network-medianizer) in order to function properly, even in the absence of governance power over the protocol.

## 2. Component Descriptions

* `StabilityFeeTreasury` - this contract tries to keep an "optimum" amount of stability fees for itself in order to make sure it can provide funds to other contracts \(or in some cases, people\) that maintain the protocol's well-being. Anyone can periodically call a function to recalculate the optimum amount of funds to keep in the treasury. Any surplus above optimum values is transferred to the `AccountingEngine`.

## 3. Risks

### Smart Contract Bugs <a id="coding-errors"></a>

* A bug in the `StabilityFeeTreasury` would potentially block other contracts from pulling funds or would incorrectly calculate the optimum amount of funds to keep in the contract \(`CDPEngine.coinBalance[stabilityFeeTreasury]`\).
* A bug could also prevent the treasury from sending extra unused resources to another address using `transferSurplusFunds()`

### Misconfiguration

* Governance might set an incorrect address as the `AccountingEngine` or could maliciously withdraw the permission of core contracts to pull funds. Governance could also allow malicious contracts to drain the treasury.

## 4. Governance Minimization

Governance can withdraw all of their power over this module if two conditions are satisfied:

1. All treasury dependent contracts were set up correctly \(can withdraw enough funds to function properly\).
2. All external actors \(if any\) have the necessary permissions to pull funds from the treasury.

The `StabilityFeeTreasury` is part of the Level 2 Gov Minimization.

