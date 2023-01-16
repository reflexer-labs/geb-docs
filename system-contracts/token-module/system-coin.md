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
* `changeData` - if `1` governance can change the `name` and/or `symbol` and no one can use `permit()`; if different than `1` governance cannot change `name` or `symbol` anymore and `permit()` can be used
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
* `modifyParameters(parameter: bytes32`, `data: uint256)` - modify the value of `changeData`
* `setName(name_: string)` - change the token's `name` if `changeData` is `1`
* `setSymbol(symbol_: string)` - change the token's `symbol` if `changeData` is `1`
* `permit(holder: address`, `spender: address`, `nonce: uint256`, `expiry: uint256`, `allowed: bool, v: uint8`, `r: bytes32`, `s: bytes32)` - approve by signature; only callable if `changeData != 1`
* `transfer(dst: address`, `amount: uint256)` - transfers coins from `msg.sender` to `dst`

## 3. Walkthrough <a id="3-key-mechanisms-and-concepts"></a>

For the most part, `coin.sol` functions as a typical ERC20 token although it has a couple of core differences:

1. `push`, `pull` & `move` are aliases for `transferFrom` in the form of `transferFrom(msg.sender, usr, amount)` , `transferFrom(usr, msg.sender, amount)` & `transferFrom(src, dst, amount)` .
2. `permit` is a signature-based approval function. This allows an end-user to sign a message which can then be relayed by another party to submit their approval. This can be useful for applications in which the end-user does not need to hold ETH to pay for gas.

## 4. Gotchas (Potential Source of User Error) <a id="4-gotchas"></a>

Unlimited allowance is a relatively common practice. This could be something used to trick a user by a malicious contract into giving access to all their Coin. This is concerning in upgradeable contracts where the contract may appear innocent until upgraded to a malicious contract.

Coin is also susceptible to the known [ERC20 race condition](https://github.com/0xProject/0x-monorepo/issues/850), but should not normally be an issue with unlimited approval. We recommend any users using the `approval` for a specific amount be aware of this particular issue and use caution when authorizing other contracts to perform transfers on their behalf.

There is a slight deviation in `transferFrom` functionality: If the `src == msg.sender` the function does not require `approval` first and treats it as a normal `transfer` from the `msg.sender` to the `dst`.

#### Built-in meta-transaction functionality of Coin

The Coin token provides offchain approval, which means that as an owner of an ETH address, you can sign a permission (using the permit() function) which basically grants allowance to another ETH address. The ETH address that you provide permission to can then take care of the execution of the transfer but has an allowance.

## 5. Failure Modes (Bounds on Operating Conditions & External Risk Factors) <a id="5-failure-modes"></a>

* N/a
