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
* `delegates(usr: address)` - a record of each account's delegate
* `checkpoints(usr: address`, `checkpoint: uint32)` - a record of vote checkpoints for each account, by index
* `numCheckpoints(usr: address)` - the number of checkpoints for each account
* `nonces(usr: address)` - a record of states for signing / validating signatures
* `allowance(src: address, dst: address)` - approvals
* `balanceOf(usr: address)` - returns the account balance of another account with _address \_owner_.
* `allowance(src: address, dst: address)`- returns the amount which _\_spender_ is still allowed to withdraw from _\_owner_.
* `DOMAIN_TYPEHASH` - the EIP-712 typehash for the contract's domain
* `DELEGATION_TYPEHASH` - the EIP-712 typehash for the delegation struct used by the contract

**Functions**

* `mint(usr: address`, `amount: uint256)` - mint coins to an address
* `burn(usr: address`, `amount: uint256)` - burn at an address
* `push(usr: address`, `amount: uint256)` - transfer
* `pull(usr: address`, `amount: uint256)`- transfer from
* `move(src: address`, `dst: address`, `amount: uint256)` - transfer from
* `approve(usr: address`, `amount: uint256)` - allow pulls and moves
* `transfer(dst: address`, `amount: uint256)` - transfers coins from `msg.sender` to `dst`
* `delegate(delegatee: address)` - delegate votes from `msg.sender` to `delegatee`
* `delegateBySig(delegatee: address`, `nonce: uint256`, `expiry: uint256`, `v: uint8`, `r: bytes32`, `s: bytes32)` - delegates votes from signatory to `delegatee`
* `getCurrentVotes(account: address) external view returns (uint256)` - gets the current votes balance for `account`
* `getPriorVotes(account: address`, `blockNumber: uint256) public view returns (uint256)` - determine the prior number of votes for an account as of a block number

**Data Structures**

* `Checkpoint` - stores a checkpoint's data. Contains:
  * `fromBlock` - the block from which that amount of votes has been marked
  * `votes` - the amount of votes

**Events**

* `Mint` - emitted when new tokens are minted. Contains:
  * `guy` - the address for which the contract prints tokens
  * `wad` - the amount of tokens printed
* `Burn` - emitted when tokens are burned. Contains:
  * `guy` - the address whose tokens are burned
  * `wad` - the amount of tokens that were burned
* `DelegateChanged` - emitted when an address changes its delegate. Contains:
  * `delegator` - the address that changes its delegate
  * `fromDelegate` - the old delegate
  * `toDelegate` - the new delegate
* `DelegateVotesChanged` - emitted when a delegate account's vote balance changes. Contains:
  * `delegate` - the delegate account
  * `previousBalance` - the delegate's previous vote balance
  * `newBalance` - the delegate's new vote balance

## 3. Walkthrough <a id="3-key-mechanisms-and-concepts"></a>

Along with the standard ERC20 token interface, the protocol token also has [DSAuth](https://github.com/reflexer-labs/ds-auth)-protected mint and burn functions.

