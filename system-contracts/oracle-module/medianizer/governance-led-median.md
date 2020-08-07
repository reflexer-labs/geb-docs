---
description: >-
  Medianizer that depends on governance token holders to whitelist off-chain
  oracles
---

# Governance Led Median

## 1. Summary <a id="1-introduction"></a>

The `GovernanceLedMedian` is an option to provide trusted reference prices for collateral types and for the system coin. It works by maintaining a whitelist of price feed contracts which are authorized to post price updates. Every time a new list of prices is received, the median of these is computed and used to update the stored value.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `contractEnabled` - settlement flag \(`1` or `0`\).
* `authorizedAccounts[usr: address]` - addresses allowed to call authed functions.
* `medianPrice` - the \(privately\) stored median price which can be read using `read()` or `getResultWithValidity()`
* `lastUpdateTime` - the block timestamp of the last `medianPrice` update.
* `symbol` - the price oracles type \(ex: ETHUSD\) / tells us what the type of asset the medianizer gives a price feed for.
* `quorum` - the minimum quorum which dictates how many addresses need to come together to `updateResult`
* `whitelistedOracles[oracle: uint256]` - mapping of oracles that are allowed to sign and submit prices to `updateResult`
* `oracleAddresses[oracle: address]` - addresses of oracles allowed to update the medianizer

**Modifiers**

* `isAuthorized` ****- checks whether an address is part of `authorizedAddresses` \(and thus can call authed functions\).

**Functions**

* `read() external view returns (uint256)` - gets a non-zero price or fails.
* `getResultWithValidity() external view returns (uint256, bool)` - gets the price and its validity.
* `updateResult(prices_: uint256[]`, `updateTimestamps_: uint256[]`, `v: uint8[]`, `r: bytes32[]`, `s: bytes32[])` - updates price using whitelisted providers.
* `addOracles(orcls: address[])`- adds an address to the writers whitelist.
* `removeOracles(orcls: address[])` - removes an address from the writers whitelist.
* `setQuorum(quorum_: uint256)` - sets the `quorum`.

**Events**

* `LogMedianPrice` - emitted when `updateResult` is called. Contains:
  * `medianPrice` - the new `medianPrice`
  * `lastUpdateTime` - the current block timestamp

## 3. Walkthrough

Authorization is a key component included in this medianizer. For example, the `medianPrice` is kept private because the intention is to only read it from the two functions `read` and `getResultWithValidity`\(in case we ever intend to add read authorization like in MCD's `OSM`\). Oracle authorization is done by calling `addOracles` or `removeOracles`.

The `updateResult` function is not under any kind of authorization. This means that anybody can call it and modify the `medianPrice` by providing valid data. "Valid data" means that at least `quorum` prices must be submitted to the contract alongside the same number of `updateTimestamps` \(array of Unix timestamps for every price\) and the `v`, `r` and `s` values which can help prove that every price was signed by an authorized oracle. Two or more prices cannot be signed by the same oracle because the contract checks for uniqueness using a `bloom` filter. 

