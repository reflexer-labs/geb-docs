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

**The ESM is intended to be used in a few potential scenarios:**

* To mitigate malicious governance
* To prevent the exploitation of a critical bug (for example one that allows collateral to be stolen)

In the case of a malicious governance attack (if the system is overned), the users will have no expectation of recovering their funds (as that would require a malicious majority to pass the required vote), and their only option is to set up an alternative fork in which the majority's funds are slashed and their funds are restored.

In other cases, the remaining FLX holders may choose to refund the ESM joiners by minting new tokens.

**Note:** Governance can disarm the ESM by calling `disableContract()`.

## 4. Gotchas (Potential Source of User Error)

### Unrecoverable of Funds

It is important for users to keep in mind that joining FLX into the ESM is irreversible—they lose it forever, regardless of whether they successfully trigger Shutdown. While it is possible that the remaining FLX holders may vote to mint new tokens for those that lose them triggering the ESM, there is no guarantee of this. In ungoverned systems this might be impossible.

### Game Theory of Funding and Firing the ESM

An entity wishing to trigger the ESM but possessing insufficient FLX to do so independently must proceed with caution. The entity could simply send FLX to the ESM to signal its desire and hope others join in; this, however, is poor strategy. Governance, whether honest or malicious, will see this, and likely move to de-authorize the ESM before the tipping point can be reached. It is clear why malicious governance would do so, but honest governance might act in a similar fashion—e.g. to prevent the system from being shut down by trolls or simply to maintain a constant threshold for ESM activation. (The ESM threshold setter automates the threshold so it reflects a pre set percentage of the protocol token's supply) If governance succeeds in this, the entity has simply lost FLX without accomplishing anything.

If an entity with insufficient FLX wishes to trigger the ESM, it is better off first coordinating with others either off-chain or ideally via a trustless smart contract. If a smart contract is used, it would be best if it employed zero-knowledge cryptography and other privacy-preserving techniques (such as transaction relayers) to obscure information such as the current amount of FLX committed and the addresses of those in support.

If an entity thinks others will join in before governance can react (e.g. if the delay for governance actions is very long), it is still possible that directly sending insufficient FLX to the ESM may work, but it carries a high degree of risk. Governance could even collude with miners to prevent `shutdownSystem` calls, etc if they suspect an ESM triggering is being organized and wish to prevent it.

## 5. Failure Modes (Bounds on Operating Conditions & External Risk Factors)

### Authorization Misconfigurations

The ESM itself does not have an isolated failure mode, but if the other parts of the system do not have proper authorization configurations (e.g. the GlobalSettlement contract does not authorize the ESM to call `disableContract()`), then the ESM's `shutdown()` method may be unable to trigger the GlobalSettlement process even if sufficient FLX has been committed to the contract.
