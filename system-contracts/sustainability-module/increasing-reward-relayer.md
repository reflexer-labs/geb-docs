---
description: Contract that relays SF rewards from the treasury to any other address
---

# Increasing Reward Relayer

## 1. Summary <a id="1-introduction-summary"></a>

The `IncreasingRewardRelayer` is a contract meant to pull funds from the `StabilityFeeTreasury` and send them to a custom address. The relayer can only be called by a specific address that requests payments.

This contract inherits functionality from the [IncreasingTreasuryReimbursement](https://docs.reflexer.finance/system-contracts/sustainability-module/increasing-treasury-reimbursement) contract.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `refundRequestor` - address that can request funds
* `lastReimburseTime` - timestamp of the last reimbursement
* `reimburseDelay` - enforced gap between reimbursements

**Functions**

* `modifyParameters` - modify contract parameters
* `reimburseCaller(feeReceiver: address)` - send SF rewards from the treasury to the `feeReceiver`

## 3. Walkthrough <a id="2-contract-details"></a>

`refundRequestor` is the only address that can call `reimburseCaller` and request a stability fee payment from the treasury.

`reimburseCaller` can only be called again after at least `reimburseDelay` seconds have passed since the last call.

