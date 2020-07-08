---
description: Governance contract for stopping OSM-like contracts
---

# FSM Governance Interface

## 1. Summary

This governance interface allows token holders to `stop()` OSM-like contracts in case of an oracle attack.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

* `owner` - the address of the contract's owner. Meant to set FSM addresses
* `onlyOwner` - modifier that checks whether the `owner` calls a function
* `authority` - contract authority, able only to stop an `OSM`
* `isAuthorized` - modifier that checks whether msg.sender can call a function
* `canCall` - checks whether an address can call a function
* `fsms` - mapping of OSM-like contracts
* `setFsm(bytes32: collateralType, address: fsm)` - set the address of an `OSM` for a specific collateral type
* `setOwner(address: owner]` - change the contract's owner
* `setAuthority(address: authority)` - change the contract's authority
* `stop(bytes32: collateralType)` - stop a collateral type's OSM

## 3. Walkthrough <a id="2-contract-details"></a>

The `owner` and the `authority` can be changed using `setOwner` and `setAuthority`. The `owner` can `setFsm`s for each collateral type and any authed address \(be it the `owner`, `authority` or another address that was whitelisted in the `authority` contract\) can `stop` any `OSM`.

