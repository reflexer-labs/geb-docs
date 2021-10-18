---
description: The protocol's resource management engine
---

# Sustainability Module

**Relevant smart contracts:**

* [StabilityFeeTreasury](https://github.com/reflexer-labs/geb/blob/master/src/single/StabilityFeeTreasury.sol)
* [FSM Wrapper](https://github.com/reflexer-labs/geb-fsm/blob/master/src/FSMWrapper.sol)
* [Increasing Treasury Reimbursement](https://github.com/reflexer-labs/geb-treasury-reimbursement/blob/master/src/reimbursement/single/IncreasingTreasuryReimbursement.sol)
* [Mandatory Fixed Treasury Reimbursement](https://github.com/reflexer-labs/geb-treasury-reimbursement/blob/master/src/reimbursement/single/MandatoryFixedTreasuryReimbursement.sol)
* [Increasing Reward Relayer](https://github.com/reflexer-labs/geb-treasury-reimbursement/blob/master/src/relayer/IncreasingRewardRelayer.sol)

## 1. Overview

The **Sustainability Module** allocates resources to actors that update critical system components such as oracles, even in the absence of governance power over the protocol.

## 2. Component Descriptions

* `StabilityFeeTreasury` - this contract tries to keep an "optimum" amount of stability fees for itself in order to make sure it can provide funds to other contracts (or in some cases, people) that maintain the protocol's well-being. Anyone can periodically call a function to recalculate the optimum amount of funds to keep in the treasury. Any surplus above optimum values is transferred to the `extraSurplusReceiver`.
* `FSMWrapper` - this contract is meant to act as a funding source for FSM-like contracts as well as an interface that allows other contracts to read data from the FSM integrated with the wrapper.
* `IncreasingTreasuryReimbursement` - this contract is meant to be inherited from and used as a way to offer an increasing stability fee reward (pulled from the SF treasury) to any address.
* `MandatoryFixedTreasuryReimbursement` - this is a contract meant to be inherited from and used as a way to offer a fixed stability fee reward (pulled from the SF treasury) to any address.
*   `IncreasingRewardRelayer` - this is a contract meant to pull funds from the `StabilityFeeTreasury` and send them to a custom address. It inherits functionality from the&#x20;

    `IncreasingTreasuryReimbursement` contract

## 3. Risks

### Smart Contract Bugs <a href="coding-errors" id="coding-errors"></a>

* A bug in the `StabilityFeeTreasury` would potentially block other contracts from pulling funds or would incorrectly calculate the optimum amount of funds to keep in the contract (`SAFEEngine.coinBalance[stabilityFeeTreasury]`). A bug could also prevent the treasury from sending extra unused resources to another address using `transferSurplusFunds()`
*   A bug in the `IncreasingTreasuryReimbursement` contract could block the execution of&#x20;

    `rewardCaller()` or it would make it impossible for someone to call `getCallerReward`
* Similar to the `IncreasingTreasuryReimbursement` contract, a bug in`MandatoryFixedTreasuryReimbursement` could block the execution of `rewardCaller()`

### Misconfiguration

* Governance might set an incorrect address as the `extraSurplusReceiver` in the `StabilityFeeTreasury` or could maliciously withdraw the permission of core contracts to pull funds. Governance could also allow malicious contracts to drain the treasury.
*   Governance might set high values for `maxRewardIncreaseDelay` and `perSecondCallerRewardIncrease` inside `IncreasingTreasuryReimbursement` and thus make&#x20;

    `getCallerReward` revert

## 4. Governance Minimization

Governance can withdraw their power over the `StabilityFeeTreasury` if two conditions are satisfied:

1. All treasury dependent contracts were set up correctly (can withdraw enough funds to function properly).
2. All external actors (if any) have the necessary permissions to pull funds from the treasury.

The `StabilityFeeTreasury` is part of the Level 2 Gov Minimization. That being said, governance should maintain control only over setting `total` allowances to their initial values for every address that's currently authorized to `pullFunds` from the treasury.

The `FSMWrapper` may need to have leftover governance (depending on how much governance wants to automate reward setting).

`IncreasingRewardRelayer` can have governance in the long run, depending on how much governance wants to remove themselves from the contract and also on what the contract is requesting rewards for.

`IncreasingTreasuryReimbursement` and `MandatoryFixedTreasuryReimbursement` are meant to be inherited by other contracts and so the contracts that inherit them will determine how much they can be governance minimized.

{% hint style="info" %}
**Keeping Governance Over **`takeFunds`

Given that `StabilityFeeTreasury.takeFunds` has very simple and clearly defined behaviour, it can be governed in the long run.&#x20;
{% endhint %}
