---
description: The guardian that decides who can mint or burn protocol tokens
---

# Protocol Token Authority

## 1. Summary <a id="1-introduction-summary"></a>

The Protocol Token Authority allows governance to specify which addresses are allowed to `mint` or `burn` protocol tokens. This is done by either setting an address as the `root` of the contract or by whitelisting an address in the `authorizedAccounts` mapping.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

* `root` - contract's controlling address
* `isRootCalling` - modifier checking if the `msg.sender` is the `root`
* `setRoot(address: usr)` - sets a new root
* `authorizedAccounts` - mapping with all authorized accounts \(accounts that can mint and burn tokens\)
* `addAuthorization` - authorize a new address
* `removeAuthorization` - de-authorize an address
* `burn` - burn your own tokens
* `burnFrom` - burn another address' tokens
* `mint` - mint new tokens
* `canCall(src: address, address, sig: bytes4)` - checks whether an address can call `mint`, `burn` or `burnFrom`

## 3. Walkthrough <a id="2-contract-details"></a>

The `root` or other `auth`ed accounts are the ones who pass the `canCall` check and thus, are able `mint`, `burn` or `burnFrom`. `root` can add or remove authorized addresses and it has complete control over the token.

