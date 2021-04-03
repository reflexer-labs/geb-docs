---
description: Setter that periodically recomputes the threshold in the ESM
---

# ESM Threshold Setter

## 1. Summary <a id="1-introduction-summary"></a>

The `ESMThresholdSetter` is meant to recompute the `threshold` of protocol tokens needed to burn and trigger settlement through the `ESM`.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `authorizedAccounts[usr: address]` - addresses allowed to call `modifyParameters()` and `disableContract()`.
* `minAmountToBurn` - the minimum amount of protocol tokens that must be burned to trigger settlement using the ESM
* `supplyPercentageToBurn` - the percentage of outstanding protocol tokens to burn in order to trigger settlement using the ESM
* `protocolToken` - The address of the protocol token
* `esm` - the address of the ESM contract

**Functions**

* `modifyParameters` - modify contract parameters
* `recomputeThreshold` - calculate and set a new protocol token threshold in the ESM

**Modifiers**

* `isAuthorized` ****- checks whether an address is part of `authorizedAddresses` \(and thus can call authed functions\).

**Events**

* `AddAuthorization` - emitted when a new address becomes authorized. Contains:
  * `account` - the new authorized account
* `RemoveAuthorization` - emitted when an address is de-authorized. Contains:
  * `account` - the address that was de-authorized
* `ModifyParameters` - emitted when a parameter is updated.

## 3. Walkthrough <a id="2-contract-details"></a>

`recomputeThreshold` can be called any time in order to recompute the threshold inside the `esm`. The computed threshold must be higher than or equal to `minAmountToBurn` and it should be a specific percentage of the outstanding supply of protocol tokens.

