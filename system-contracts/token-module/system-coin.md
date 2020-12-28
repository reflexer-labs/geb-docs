---
description: ERC20 representation of the coins backed by system collateral
---

# System Coin

## 1. Summary <a id="1-introduction-summary"></a>

The `Coin` contract is the user-facing ERC20 token maintaining the accounting for external system coin balances. Most functions are standard for a token with changing supply, but it also has notable features such as the ability to approve transfers based on signed messages.

## 2. Contract Details & Functions <a id="2-contract-details"></a>

**Variables**

* `name`
* `symbol`
* `version`
* `decimals`
* `changeData` - if `0` governance can't change the `name` and/or `symbol` and no one can use `permit()`; if greater than zero governance can change them
* `totalSupply` - total coin supply
* `balanceOf(usr: address)` - user balance
* `allowance(src: address, dst: address)` - approvals
* `nonces(usr: address)` - permit nonce
* `wad` - fixed point decimal with 18 decimals \(for basic quantities, e.g. balances\).

**Functions**

* `mint(usr: address`, `amount: uint256)` - mint coins to an address
* `burn(usr: address`, `amount: uint256)` - burn at an address
* `push(usr: address`, `amount: uint256)` - transfer
* `pull(usr: address`, `amount: uint256)`- transfer from
* `move(src: address`, `dst: address`, `amount: uint256)` - transfer from
* `approve(usr: address`, `amount: uint256)` - allow pulls and moves
* `permit(holder: address`, `spender: address`, `nonce: uint256`, `expiry: uint256`, `allowed: bool, v: uint8`, `r: bytes32`, `s: bytes32)` - approve by signature
* `transfer(dst: address`, `amount: uint256)` - transfers coins from `msg.sender` to `dst`

## 3. Walkthrough <a id="3-key-mechanisms-and-concepts"></a>

For the most part, `coin.sol` functions as a typical ERC20 token although it has a couple of core differences:

1. `push`, `pull` & `move` are aliases for `transferFrom` in the form of `transferFrom(msg.sender, usr, amount)` , `transferFrom(usr, msg.sender, amount)` & `transferFrom(src, dst, amount)` .
2. `permit` is a signature-based approval function. This allows an end-user to sign a message which can then be relayed by another party to submit their approval. This can be useful for applications in which the end-user does not need to hold ETH to pay for gas.

