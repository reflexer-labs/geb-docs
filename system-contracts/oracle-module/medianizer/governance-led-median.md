---
description: >-
  Medianizer that depends on governance token holders to whitelist off-chain
  oracles
---

# Governance Led Median

## 1. Summary <a href="#1-introduction" id="1-introduction"></a>

The `GovernanceLedMedian` is an option to provide trusted reference prices for collateral types and for the system coin. It works by maintaining a whitelist of price feed contracts which are authorized to post price updates. Every time a new list of prices is received, the median of these is computed and used to update the stored value.

## 2. Contract Variables & Functions <a href="#2-contract-details" id="2-contract-details"></a>

**Variables**

* `contractEnabled` - settlement flag (`1` or `0`).
* `authorizedAccounts[usr: address]` - addresses allowed to call authed functions.
* `medianPrice` - the (privately) stored median price which can be read using `read()` or `getResultWithValidity()`
* `lastUpdateTime` - the block timestamp of the last `medianPrice` update.
* `symbol` - the price oracles type (ex: ETHUSD) / tells us what the type of asset the medianizer gives a price feed for.
* `quorum` - the minimum quorum which dictates how many addresses need to come together to `updateResult`
* `whitelistedOracles[oracle: uint256]` - mapping of oracles that are allowed to sign and submit prices to `updateResult`
* `oracleAddresses[oracle: address]` - addresses of oracles allowed to update the medianizer

**Modifiers**

* `isAuthorized` **** - checks whether an address is part of `authorizedAddresses` (and thus can call authed functions).

**Functions**

* `read() external view returns (uint256)` - gets a non-zero price or fail.
* `getResultWithValidity() external view returns (uint256, bool)` - gets the price and its validity.
* `updateResult(prices_: uint256[]`, `updateTimestamps_: uint256[]`, `v: uint8[]`, `r: bytes32[]`, `s: bytes32[])` - updates price using whitelisted providers.
* `addOracles(orcls: address[])`- adds an address to the writers whitelist.
* `removeOracles(orcls: address[])` - removes an address from the writers whitelist.
* `setQuorum(quorum_: uint256)` - sets the `quorum`.

**Events**

* `AddAuthorization` - emitted when a new address becomes authorized. Contains:
  * `account` - the new authorized account
* `RemoveAuthorization` - emitted when an address is de-authorized. Contains:
  * `account` - the address that was de-authorized
* `AddOracles` - emitted when an authed address adds new oracles that can update the price feed. Contains:
  * `orcls` - array of oracles that will be added
* `RemoveOracles` - emitted when an authed address removes previously whitelisted oracles. Contains:
  * `orcls` - array of oracles that will be removed
* `SetQuorum` - emitted when a new quorum is set. Contains:
  * `quorum` - the new median quorum
* `UpdateResult` - emitted when `updateResult` is called. Contains:
  * `medianPrice` - the new `medianPrice`
  * `lastUpdateTime` - the current block timestamp

## 3. Walkthrough

Authorization is a key component included in this medianizer. For example, the `medianPrice` is kept private because the intention is to only read it using `read` and `getResultWithValidity`(in case we ever intend to add read authorization like in MCD's `OSM`). Oracle authorization is done by calling `addOracles` or `removeOracles`.

`updateResult` is not under any kind of authorization. This means that it can be called by anyone who provides valid data. By "valid data" we mean that at least `quorum` prices must be submitted to the contract alongside the same number of `updateTimestamps` (array of Unix timestamps for every price) and the `v`, `r` and `s` values which can help prove that every price was signed by an authorized oracle. Two or more prices cannot be signed by the same oracle because the contract checks for uniqueness using a `bloom` filter.&#x20;

## 4. Gotchas

#### **Emergency Oracles**

* They can shutdown the price feed but cannot bring it back up. Bringing the price feed back up requires governance to step in.

#### **Price Freeze**

* If you void the oracles Ethereum module, the idea is that you cannot interact with any SAFE that depends on that collateralType.
  * **Example:** ETHUSD shutdown (can still add collateral and pay back debt - increases safety) but you cannot do anything that increases risk (decreases safety - remove collateral, generate coin, etc.) because the system would not know if you would be undercollateralized.

#### **Governance Led Oracles Require a lot of Upkeep**

* They need to keep all relayers functioning.
* The community would need to self-police (by looking at each price signer, etc.) if any of them needs to be replaced. They would need to make sure they are constantly being called every hour (for every hour, a transaction gets sent to the OSM, which means that a few transactions have already been sent to the median to update it as well. In addition, there would need to be a transaction sent to the `oracleRelayer`, as GEB operates in a pool-type method (doesn't update the system/write to it, you tell it to read it from the OSM).

#### **There is nothing preventing from `addAuthorization`'ing two prices signers that start with the same address**

* The only thing that this prevents is that you cannot have more than 256 oracles but we don't expect to ever have that many, so it is a hard limit. However, Governance needs to be sure that whoever they are voting in anyone that they have already voting in before with the same two first characters.
* An example of what a governance proposal would look like in this case:
  * We are adding a new oracle and are proposing (the Foundation) a list of signers (that have been used in the past) and we already have an oracle but want to add someone new (e.g. Dharma or dydx). We would say that they want to be price signers, so these are their addresses and we want to auth those two addresses. They would vote for that, and we would need to keep a list of the already existing addresses and they would need to create an address that doesn't conflict with the existing ones.

## 5. Failure Modes (Bounds on Operating Conditions & External Risk Factors)

By design, there is currently no way right now to turn off the oracle (failure or returns false) if all the oracles come together and sign a price of zero. This would result in the price being invalid and would return false on `getResultWithValidity`, telling us to not trust the value.