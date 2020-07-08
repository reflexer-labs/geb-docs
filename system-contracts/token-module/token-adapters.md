---
description: The gateways for collateral and system coins to join or exit the system
---

# Token Adapters

## 1. Summary <a id="1-introduction-summary"></a>

There are three main token adapter types: `CollateralJoin`, `ETHJoin` and `CoinJoin`: 

* `CollateralJoin` - allows standard ERC20 tokens to be deposited for use with the system. 
* `ETHJoin` - allows native Ether to be used with the system. 
* `CoinJoin` - allows users to withdraw their system coins from the protocol into a standard ERC20 token.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

* `cdpEngine` - storage of the `CDPEngine`’s address.
* `collateralType` - id of the `CollateralType` for which a `CollateralJoin` is created for.
* `collateral` - the address of the `collateralType` for transferring.
* `systemCoin` - the address of the `Coin` token.
* `ONE` - a 10^27 uint used for math in `CoinJoin`.
* `contractEnabled` - an access flag for the adapter.
* `decimals` - decimals for the collateral type.
* `join` - join tokens into the system
* `exit` - exit tokens from the system
* `disableContract` - disable the adapter and only allow `exit`s

## 3. Walkthrough <a id="3-key-mechanisms-and-concepts"></a>

All adapters contracts serve a similar purpose. They manage the flow of collateral and system coins in and out of the system using `join` and `exit`. `CollateralJoin` is meant to change the `CDPEngine.tokenCollateral` and `CoinJoin` changes `CDPEngine.coinBalance`s.



