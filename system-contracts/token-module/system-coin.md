---
description: ERC20 representation of the coins backed by system collateral
---

# System Coin

## 1. Summary <a id="1-introduction-summary"></a>

The `Coin` contract is the user-facing ERC20 token maintaining the accounting for external system coin balances. Most functions are standard for a token with changing supply, but it also has notable features such as the ability to approve transfers based on signed messages.

## 2. Contract Details & Functions <a id="2-contract-details"></a>

* `mint` - mint coins to an address
* `burn` - burn at an address
* `push` - transfer
* `pull` - transfer from
* `move` - transfer from
* `approve` - allow pulls and moves
* `permit` - approve by signature
* `name`
* `symbol`
* `version`
* `decimals`
* `totalSupply` - total coin supply
* `balanceOf(usr: address)` - user balance
* `allowance(src: address, dst: address)` - approvals
* `nonces(usr: address)` - permit nonce
* `wad` - fixed point decimal with 18 decimals \(for basic quantities, e.g. balances\).

## 3. Walkthrough <a id="3-key-mechanisms-and-concepts"></a>

For the most part, `coin.sol` functions as a typical ERC20 token although it has a couple of core differences:

1. `push`, `pull` & `move` are aliases for `transferFrom` calls in the form of `transferFrom(msg.sender, usr, amount)` , `transferFrom(usr, msg.sender, amount)` & `transferFrom(src, dst, amount)` .
2. `permit` is a signature-based approval function. This allows an end-user to sign a message which can then be relayed by another party to submit their approval. This can be useful for applications in which the end-user does not need to hold ETH to pay for gas.

