---
description: Contract that relays SF rewards from the treasury to any other address
---

# Increasing Reward Relayer

## 1. Summary <a id="1-introduction-summary"></a>

The `IncreasingRewardRelayer` is a contract meant to pull funds from the `StabilityFeeTreasury` and send them to a custom address. The relayer can only be called by a specific address that requests payments.

This contract inherits functionality from the [IncreasingTreasuryReimbursement](https://docs.reflexer.finance/system-contracts/sustainability-module/increasing-treasury-reimbursement) contract.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `authorizedAccounts[usr: address]` - `addAuthorization`/`removeAuthorization` - auth mechanisms
* `fixedReward` - the fixed reward sent by the `treasury` to a fee receiver
* `treasury` - stability fee treasury contract

**Functions**

