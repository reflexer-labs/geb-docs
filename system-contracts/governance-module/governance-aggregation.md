---
description: Basic aggregator for setting multiple system parameters at once
---

# Governance Aggregation

## 1. Summary <a id="1-introduction-summary"></a>

The `GovernanceAggregator` is a basic contract that allows token holders \(or the multisig that controls the system\) to set multiple protocol parameters at once or add/remove multiple `authorizedAccounts` in one transaction.

##  2. Contract Variables & Functions

* `authorizedAccounts[usr: address]`, `addAuthorization`/`removeAuthorization`/`isAuthorized` - auth mechanisms
* `addAuthMany(auth: address, targetContracts: address[])` - add an address as an `authorizedAccount` in multiple contracts
* `removeAuthMany(auth: address, targetContracts: address[])` - remove an `authorizedAccount` from multiple contracts
* `taxSingleAndModifyParameters` - collect stability fees from one collateral type in `TaxCollector` and then change a parameter
* `taxManyAndModifyParameters` - collect stability fees from multiple collateral types in `TaxCollector` and then change a parameter
* `changeMultiCollateralTypeParams` - change multiple collateral type related parameters from multiple smart contracts
* `changeMultiUintParams` - change multiple `uint256` parameters from multiple smart contracts
* `changeMultiAddressParams` - change multiple `address` parameters from multiple smart contracts

## 3. Walkthrough <a id="2-contract-details"></a>

All the aggregator's functions allow `authorizedAccounts` to set many parameters at once in the core GEB contracts. It is extremely useful when governance needs to set a `stabilityFee` related parameter \(but first has to collect fees from one or more collateral types\) or when it wants to add/remove their power over many contracts at once.

