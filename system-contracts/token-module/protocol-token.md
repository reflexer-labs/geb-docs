---
description: The protocol's recapitalization source
---

# Protocol Token

## 1. Summary <a id="1-introduction-summary"></a>

The protocol token is a [DsDelegateToken](https://github.com/reflexer-labs/ds-token/blob/master/src/delegate.sol) that provides logic for burning and authorized minting of new tokens as well as delegation capabilities.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `guy` - user address
* `wad` - a quantity of tokens, usually as a fixed point integer with 10^18 decimal places.
* `dst` - refers to the destination address.
* `name` - returns the name of the token - e.g. "MyToken".
* `symbol` - token symbol.
* `decimals` - returns the number of decimals the token uses.
* `totalSupply` - returns the total token supply.
* `balanceOf(usr: address)` - user balance
* `allowance(src: address, dst: address)` - approvals
* `balanceOf(usr: address)` - returns the account balance of another account with _address \_owner_.
* `allowance(src: address, dst: address)`- returns the amount which _\_spender_ is still allowed to withdraw from _\_owner_.

**Functions**

* `mint(usr: address`, `amount: uint256)` - mint coins to an address
* `burn(usr: address`, `amount: uint256)` - burn at an address
* `push(usr: address`, `amount: uint256)` - transfer
* `pull(usr: address`, `amount: uint256)`- transfer from
* `move(src: address`, `dst: address`, `amount: uint256)` - transfer from
* `approve(usr: address`, `amount: uint256)` - allow pulls and moves
* `transfer(dst: address`, `amount: uint256)` - transfers coins from `msg.sender` to `dst`

## 3. Walkthrough <a id="3-key-mechanisms-and-concepts"></a>

Along with the protocol token having a standard ERC20 token interface, it also has [DSAuth](https://github.com/reflexer-labs/ds-auth)-protected mint and burn functions.

