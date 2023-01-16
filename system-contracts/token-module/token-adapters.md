---
description: The gateways for collateral and system coins to join or exit the system
---

# Token Adapters

## 1. Summary <a id="1-introduction-summary"></a>

There are three main token adapter types: `CollateralJoin`, `ETHJoin` and `CoinJoin`:

* `CollateralJoin` - allows standard ERC20 tokens to be deposited for use with the system.
* `ETHJoin` - allows native Ether to be used with the system.
* `CoinJoin` - allows users to withdraw their system coins from the protocol into a standard ERC20 token \(and vice-versa\).

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `safeEngine` - storage of the `SAFEEngine`’s address.
* `collateralType` - id of the `CollateralType` for which a `CollateralJoin` is created for.
* `collateral` - the address of the `collateralType` for transferring.
* `systemCoin` - the address of the `Coin` token.
* `contractEnabled` - an access flag for the adapter.
* `decimals` - decimals for the collateral type.

**Functions**

* `join` - join tokens into the system
* `exit` - exit tokens from the system
* `disableContract` - disable the adapter and only allow `exit`s

Every `token adapter` contract has 4 public functions: a constructor, `join`, `exit`, and `disableContract`. The constructor is used on contract initialization and sets the core variables of that `token adapter` contract. `Join` and `exit` are both true to their names. `Join` provides a mechanism for users to add the given token type to the `SafeEngine`. It has slightly different logic in each variation, but generally resolves down to a `transfer` and a function call in the `SafeEngine`. `Exit` is very similar, but instead allows the the user to remove their desired token from the `SafeEngine`. `disableContract` allows the adapter to be drained (allows tokens to move out but not in).

## 3. Walkthrough <a id="3-key-mechanisms-and-concepts"></a>

The `CollateralJoin` contract serves a very specified and singular purpose which is relatively abstracted away from the rest of the core smart contract system. When a user desires to enter the system and interact with the `geb` contracts, they must use one of the `CollateralJoin` contracts. After they have finished with the `geb` contracts, they must call `exit` to leave the system and take out their tokens. When the `CollateralJoin` gets `disableContract`d by an `auth`ed address, it can `exit` collateral from the SafeEngine but it can no longer `join` new collateral.

User balances for collateral tokens added to the system via `disableContract` are accounted for in the `SafeEngine` as `tokenCollateral` according to collateral type `CollateralType` until they are converted into locked collateral tokens (`Safe.lockedCollateral`) so the user can draw system coin.

The `CoinJoin` contract serves a similar purpose. It manages the exchange of Coin that is tracked in the `SafeEngine` and ERC-20 Coin that is tracked by `Coin.sol`. After a user draws Coin against their collateral, they will have a balance in `CoinJoin.coinBalance`. This Coin balance can be `exit`' ed from the SafeEngine using the `CoinJoin` contract which holds the balance of `SafeEngine.coinBalance` and mint's ERC-20 Coin. When a user wants to move their Coin back into the `SafeEngine` accounting system (to pay back debt, participate in auctions, pack `coinBag`'s in the `GlobalSettlement`, or utilize the CoinSavingsAccount, etc), they must call `CoinJoin.join`. By calling `CoinJoin.join` this effectively `burn`'s the ERC-20 Coin and transfers `SafeEngine.coinBalance` from the `CoinJoin`'s balance to the User's account in the `SafeEngine`. Under normal operation of the system, the `Coin.totalSupply` should equal the `SafEngine.coinBalance(CoinJoin)` balance. When the `CoinJoin` contract gets `disableContract`'d by an `auth`'ed address, it can move Coin back into the SafeEngine but it can no longer `exit` Coin from the SafeEngine.

## 4. Gotchas (Potential source of user error)  <a id="4-gotchas"></a>

The main source of user error with the `TokenAdapter` contract is that Users should never `transfer` tokens directly to the contracts, they **must** use the `join` functions or they will not be able to retrieve their tokens.

There are limited sources of user error in the `TokenAdapter` contract system due to the limited functionality of the system. Barring a contract bug, should a user call `join` by accident they could always get their tokens back through the corresponding `exit` call on the given `join` contract.

The main issue to be aware of here would be a well-executed phishing attack. As the system evolves and potentially more `TokenAdapter` contracts are created, or more user interfaces are made, there is the potential for a user to have their funds stolen by a malicious `TokenAdapter` contract which does not actually send tokens to the `SafeEngine`, but instead to some other contract or wallet.

## 5. Failure Modes (Bounds on Operating Conditions & External Risk Factors) <a id="5-failure-modes"></a>

**There could potentially be a `SafeEngine` upgrade that would require new `TokenAdapter` contracts to be created.**

If a `collateral` contract were to go through a token upgrade or have the tokens frozen while a user's collateral was in the system, there could potentially be a scenario in which the users were unable to redeem their collateral after the freeze or upgrade was finished. This scenario likely presents little risk though because the token going through this upgrade would more than likely want to work alongside the community to be sure this was not an issue.