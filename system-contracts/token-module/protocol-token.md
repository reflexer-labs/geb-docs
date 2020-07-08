---
description: The protocol's recapitalization source
---

# Protocol Token

## 1. Summary <a id="1-introduction-summary"></a>

The protocol token is a [DsToken](https://github.com/reflexer-labs/ds-token.git). It is an ERC20 token that provides logic for burning and authorized minting of new tokens.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

* `guy` - user address
* `wad` - a quantity of tokens, usually as a fixed point integer with 10^18 decimal places.
* `dst` - refers to the destination address.
* `mint` - credit tokens at an address whilst simultaneously increasing `totalSupply` \(requires auth\).
* `burn` - debit tokens at an address whilst simultaneously decreasing `totalSupply` \(requires auth\).
* `push` - transfer an amount from msg.sender to a given address.
* `pull` - transfer an amount from a given address to msg.sender \(requires trust or approval\).
* `move` - transfer an amount from a given src address to a given dst address \(requires trust or approval\).
* `name` - returns the name of the token - e.g. "MyToken".
* `symbol` - token symbol.
* `decimals` - returns the number of decimals the token uses.
* `transfer` - transfers `_value` amount of tokens to _address_ `_to`
* `transferFrom` - transfers `_value` amount of tokens from address `_from` to address `_to`
* `approve` - allows _\_spender_ to withdraw from your account multiple times, up to the `_value` amount.
* `totalSupply` - returns the total token supply.
* `balanceOf` - returns the account balance of another account with _address \_owner_.
* `allowance` - returns the amount which _\_spender_ is still allowed to withdraw from _\_owner_.

## 3. Walkthrough <a id="3-key-mechanisms-and-concepts"></a>

Along with the protocol token having a standard ERC20 token interface, it also has [DSAuth](https://github.com/reflexer-labs/ds-auth)-protected mint and burn functions.

