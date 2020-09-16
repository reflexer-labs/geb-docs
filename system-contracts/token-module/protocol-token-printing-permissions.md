---
description: >-
  Contract allowing multiple independent debt auction houses to print protocol
  tokens
---

# Protocol Token Printing Permissions

## 1. Summary <a id="1-introduction-summary"></a>

`GebPrintingPermissions` allows governance to whitelist multiple independent GEBs to print the same protocol token. It imposes delays for adding and removing permissions and it can also allow specific systems to print tokens indefinitely. 

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `authorizedAccounts[usr: address]` - addresses allowed to cover or uncover systems
* `allowedSystems[system: address]` - data about a system that is allowed to print protocol tokens
* `usedAuctionHouses[house: address]` - mapping that shows whether a `DebtAuctionHouse` is allowed to print tokens or not
* `unrevokableRightsCooldown` - minimum time \(in seconds\) after which as system will have indefinite permission to print protocol tokens
* `denyRightsCooldown` - cooldown that must pass between calling `startUncoverSystem` and `endUncoverSystem`
* `addRightsCooldown` - cooldown for quickly removing cover from a freshly added system 
* `coveredSystems` - the current number of systems covered by the protocol token
* `protocolTokenAuthority` - the authority contract dictating who can print tokens
* `AUCTION_HOUSE_TYPE` - debt auction house identifier

**Data Structures**

* `SystemRights` - struct that stores data about each `AccountingEngine` \(system\). Contains:
  * `covered` - whether this system is covered or not
  * `revokeRightsDeadline` - deadline after which governance can no longer remove printing permissions from the system
  * `uncoverCooldownEnd` - cooldown after which governance can call `endUncoverSystem` and uncover this system
  * `withdrawAddedRightsDeadline` - cooldown during which governance can uncover a system without having to wait until `uncoverCooldownEnd` passed
  * `previousDebtAuctionHouse` - the previous `DebtAuctionHouse` connected to the `AccountingEngine`
  * `currentDebtAuctionHouse` - the current `DebtAuctionHouse` connected to the `AccountingEngine`

**Modifiers**

* `isAuthorized` ****- checks whether an address is part of `authorizedAddresses`.

**Functions**

* `addAuthorization(usr: address)` - add an address to `authorizedAddresses`.
* `removeAuthorization(usr: address)` - remove an address from `authorizedAddresses`.

**Events**

