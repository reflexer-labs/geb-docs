---
description: The guardian that decides who can mint or burn protocol tokens
---

# Protocol Token Authority

## 1. Summary <a id="1-introduction-summary"></a>

The `ProtocolTokenAuthority` allows governance to specify which addresses are allowed to `mint` or `burn` protocol tokens. This is done by either setting an address as the `root` of the contract or by whitelisting an address in the `authorizedAccounts` mapping.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `root` - contract's root controlling address.
* `owner` - the second most powerful address controlling the contract. It cannot set the `root` but it can set another `owner`.
* `authorizedAccounts[account: address]` - mapping of addresses that are allowed to print protocol tokens.

**Modifiers**

* `isRootCalling` - modifier checking if the `msg.sender` is the `root`
* `isRootOrOwnerCalling` - modifier checking if the `msg.sender` is the `root` or the `owner`

**Functions**

* `setRoot(usr: address)` - sets a new root
* `setOwner(usr: address)` - sets a new owner
* `addAuthorization(usr: address)` - authorize a new address to print tokens
* `removeAuthorization(usr: address)` - de-authorize an address from printing tokens
* `canCall(src: address, address, sig: bytes4)` - checks whether an address can call `mint`, `burn` or `burnFrom` inside the protocol token

## 3. Walkthrough <a id="2-contract-details"></a>

`auth`ed accounts, owner and root are the ones who pass the `canCall` check and thus, are able `mint`, `burn` and `burnFrom`. `root` and `owner` can add or remove authorized addresses. `owner` cannot set a new `root` but can only change the `owner`. `root` has complete power and can change both the `root` and the `owner`. 

