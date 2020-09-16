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
* unrevokableRightsCooldown -
* denyRightsCooldown -
* addRightsCooldown -
* coveredSystems -
* protocolTokenAuthority -
* AUCTION\_HOUSE\_TYPE -

