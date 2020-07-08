---
description: >-
  Medianizer that depends on governance token holders to whitelist off-chain
  oracles
---

# Governance Led Median

## 1. Summary <a id="1-introduction"></a>

The `GovernanceLedMedian` is one option to provide trusted reference prices for collateral types and for the system coin. It works by maintaining a whitelist of price feed contracts which are authorized to post price updates. Every time a new list of prices is received, the median of these is computed and used to update the stored value.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

* `authorizedAccounts [usr: address]` - `addAuthorization`/`removeAuthorization`/`isAuthorized` - auth mechanisms
* `read` - gets a non-zero price or fails.
* `getResultWithValidity` - gets the price and its validity.
* `updateResult` - updates price using whitelisted providers.
* `addOracles`- adds an address to the writers whitelist.
* `removeOracles` - removes an address from the writers whitelist.
* `setQuorum` - sets the `quorum`.
* `oracleAddresses(usr: address)` - `medianPrice` writers whitelist / signers of the prices \(whitelisted via governance / the authorized parties\).
* `medianPrice` - the price \(private\) must be read with `read()` or `getResultWithValidity()`
* `lastUpdateTime` - the Block timestamp of last price `medianPrice` update.
* `symbol` - the price oracles type \(ex: ETHUSD\) / tells us what the type of asset is.
* `quorum` - the minimum writers quorum for `getResultWithValidity`/ min number of valid messages you need to have to update the price.

## 3. Walkthrough

Authorization is a key component included in this medianizer. For example, the `medianPrice` is kept private because the intention is to only read it from the two functions `read` and `getResultWithValidity`\(in case we ever intend to add read authorization like in MCD's `OSM`\). Oracle authorization is done by calling `addOracles` or `removeOracles`.

The `updateResult` function is not under any kind of authorization. This means that anybody can call it and modify the `medianPrice` by providing valid data. "Valid data" means that at least `quorum` prices must be submitted to the contract alongside the same number of `updateTimestamps` \(array of Unix timestamps for every price\) and the `v`, `r` and `s` values which can help prove that every price was signed by an authorized oracle. Two or more prices cannot be signed by the same oracle because the contract checks for uniqueness using a `bloom` filter. 

