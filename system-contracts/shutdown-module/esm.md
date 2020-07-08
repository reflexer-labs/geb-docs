---
description: Trigger global settlement by burning protocol tokens
---

# ESM

## 1. Summary <a id="1-introduction-summary"></a>

The Emergency Shutdown Module \(ESM\) is a contract with the ability to settle a GEB. Settlement is triggered after at least `triggerThreshold` protocol tokens are deposited in the contract and subsequently burned.

## 2. Contract Variables & Functions

* `authorizedAccounts[usr: address]`, `addAuthorization`/`removeAuthorization`/`isAuthorized` - auth mechanisms
* `protocolToken` - address of the token that must be deposited in the `ESM` in order to settle the system
* `globalSettlement` - address of the global settlement contract
* `tokenBurner` - contract that will receive all deposited protocol tokens and will then burn them
* `triggerThreshold` - minimum amount of tokens that need to be deposited in order to shut down the system
* `settled` - flag that indicates whether global settlement has already been trigerred
* `burntTokens` - mapping that shows how many tokens have been deposited by each address
* `totalAmountBurnt` - total amount of tokens that have been burnt up until this point
* `modifyParameters` - change the `triggerThreshold`. Meant to be used by another contract
* `shutdown` - trigger settlement. Throws if there weren't enough tokens burned
* `burnTokens` - deposit tokens in order to be burned

## 3. Walkthrough <a id="2-contract-details"></a>

The `ESM`is meant to be used in order to prevent an attacker from exploiting a vulnerability in the system \(e.g stealing all the collateral\) or to mitigate malicious governance.

If governance wishes to trigger shutdown, they must first burn deposit and burn at least`triggerThreshold` protocol tokens in the ESM. After enough tokens are deposited, the ESM's `shutdown()` method can be called by anyone. `shutdown()` calls `GlobalSettlement.shutdownSystem()` which in turn starts the settlement procedure.

**Note:** `triggerThreshold` can be changed using `modifyParameters`. This is to make sure that the threshold is always set to a percentage of the outstanding supply of coins. The change should only be done by an autonomous, immutable smart contract.

