---
description: Trigger global settlement by burning protocol tokens
---

# ESM

## 1. Summary <a href="#1-introduction-summary" id="1-introduction-summary"></a>

The Emergency Shutdown Module (ESM) is a contract with the ability to settle a GEB. Settlement is triggered after at least `triggerThreshold` protocol tokens are deposited in the contract and subsequently burned.

## 2. Contract Variables & Functions

**Variables**

* `authorizedAccounts[usr: address]`, `addAuthorization`/`removeAuthorization`/`isAuthorized` - auth mechanisms
* `protocolToken` - address of the token that must be deposited in the `ESM` in order to settle the system
* `globalSettlement` - address of the global settlement contract
* `thresholdSetter` - address (different from `authorizedAccounts`) that is able to set the`triggerThreshold`
* `tokenBurner` - contract that will receive all deposited protocol tokens and will then burn them
* `triggerThreshold` - minimum amount of tokens that need to be deposited in order to shut down the system
* `settled` - flag that indicates whether global settlement has already been trigerred

**Modifiers**

* `isAuthorized` **** - checks whether an address is part of `authorizedAddresses` (and thus can call authed functions).

**Functions**

* `modifyParameters` - change the `triggerThreshold` as well as the `thresholdSetter`.
* `shutdown` - burns `triggerThreshold` protocol tokens and triggers settlement

## 3. Walkthrough <a href="#2-contract-details" id="2-contract-details"></a>

The `ESM`is meant to be used in order to prevent an attacker from exploiting a vulnerability in the system (e.g stealing all the collateral) or to mitigate malicious governance.

If governance wishes to trigger shutdown, they must burn at least`triggerThreshold` protocol tokens and then automatically trigger settlement. Both actions are executed using `shutdown()` which can be called by anyone. `shutdown()` calls `GlobalSettlement.shutdownSystem()` which in turn starts the settlement procedure.

**Note:** `triggerThreshold` can be changed using `modifyParameters` either by governance or by `thresholdSetter` which can be an autonomous smart contract. This ensures that the threshold is always set to a specific percentage of the outstanding supply of protocol tokens.
