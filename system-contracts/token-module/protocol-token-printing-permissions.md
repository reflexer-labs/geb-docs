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
* `modifyParameters(parameter: bytes32, data: uint256)` - modify a `uint256` parameter
* `giveUpAuthorityRoot()` - give up the perinting permission's position of `root` inside the `ProtocolTokenAuthority`
* `giveUpAuthorityOwnership` - give up the perinting permission's position of `owner` inside the `ProtocolTokenAuthority`
* `coverSystem(accountingEngine: address)` - cover a new system / allow the `DebtAuctionHouse` set inside `accountingEngine` to print protocol tokens
* `startUncoverSystem(accountingEngine: address)` - start to uncover a system / withdraw the permission of the previous and current `DebtAuctionHouse`s that were associated with `accountingEngine`
* `abandonUncoverSystem(accountingEngine: address)` - abandon the uncovering of a specific system
* `endUncoverSystem(accountingEngine: address)` - end the uncovering process for a system
* `updateCurrentDebtAuctionHouse(accountingEngine: address)` - update the `currentDebtAuctionHouse` of a specific system and set the old current auction house as the 

  `previousDebtAuctionHouse`

* `removePreviousDebtAuctionHouse(accountingEngine: address)` - remove the permission to print protocol tokens from an `accountingEngine`'s `previousDebtAuctionHouse`
* `proposeIndefinitePrintingPermissions(accountingEngine: address`, `freezeDelay: uint256)` - propose a deadline after which the `accountingEngine` / system cannot be denied printing permissions

**Events**

* `AddAuthorization` - emitted when a new address becomes authorized. Contains:
  * `account` - the new authorized account
* `RemoveAuthorization` - emitted when an address is de-authorized. Contains:
  * `account` - the address that was de-authorized
* `ModifyParameters` - emitted when a `uint256` ****parameter is updated.
* `GiveUpAuthorityRoot` - emitted when `giveUpAuthorityRoot` is called.
* `GiveUpAuthorityOwnership` -  emitted when `giveUpAuthorityOwnership` is called.
* `RevokeDebtAuctionHouses` - emitted when both the current and the previous `DebtAuctionHouse`s are denied printing permissions. Contains:
  * `accountingEngine` - the accounting engine whose debt auction houses are denied permissions
  * `currentHouse` - the current debt auction house
  * `previousHouse` - the previous debt auction house
* `CoverSystem` - emitted when a new system is covered / allowed to print tokens. Contains:
  * `accountingEngine` - the accounting engine from the system that is being covered
  * `debtAuctionHouse` - the debt auction house set inside the `accountingEngine`
  * `coveredSystems` - the new number of systems being covered
  * `withdrawAddedRightsDeadline` - the deadline after which governance can no longer quickly uncover a system without calling `startUncoverSystem` and `endUncoverSystem`
* `StartUncoverSystem` - emitted when governance starts to uncover a system. Contains:
  * 
* AbandonUncoverSystem -
* EndUncoverSystem -
* UpdateCurrentDebtAuctionHouse -
* RemovePreviousDebtAuctionHouse -
* ProposeIndefinitePrintingPermissions -

