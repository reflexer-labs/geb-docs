---
description: Tools for achieving consensus
---

# Governance Module

**Relevant smart contracts:**

* [Gnosis Multisig](https://github.com/reflexer-labs/geb-basic-multisig/blob/master/src/MultisigWallet.sol)
* [Governance Aggregation](https://github.com/reflexer-labs/geb-governance-aggregation/blob/master/src/GebBasicGovernanceAggregation.sol)
* [VoteQuorum](https://github.com/reflexer-labs/ds-vote-quorum/blob/master/src/VoteQuorum.sol)
* [DSPause](https://github.com/reflexer-labs/ds-pause/blob/master/src/pause.sol)

## 1. Overview

The **Governance Module** is a set of smart contracts that governance can use to modify and upgrade GEB. The module has both multisignature and voting based contracts that give governance flexibility in terms of how the protocol is upgraded.

## 2. Component Descriptions

* `Gnosis Multisig` - this is a simple [multisignature wallet](https://github.com/gnosis/MultiSigWallet/blob/master/contracts/MultiSigWallet.sol) made by Gnosis
* `Governance Aggregation` - this aggregator allows governance to change parameters in multiple core contracts at once, as well as add/remove authorized addresses to the core system. It is useful especially if the system is launched with a multisig as the main admin and the contract owners want to easily switch to voting based governance later on
* `VoteQuorum` - the quorum allows protocol token holders to vote on parameter changes and system upgrades. Each address' voting power is determined by the amount of tokens that they have locked in the contract. The voting mechanism is [approval voting](https://en.wikipedia.org/wiki/Approval_voting).
* `DSPause` - the pause contract enforces a delay between scheduling a transaction \(coming from `VoteQuorum` or the multisig\) and executing it.

## 3. Risks

### Governance Attack

Malicious governance may want to extract all the collateral from the system or generate a high amount of debt that is not backed by collateral. There are two possible solutions to this problem:

1. Eliminate governance over most core system components \(especially the ones that handle collateral balances\).
2. Use `DSPause` and add a delay to every transaction coming from either the multisig or `VoteQuorum`.

The second point is especially important because `VoteQuorum` can be [flashloan attacked](https://medium.com/coinmonks/how-to-turn-20m-into-340m-in-15-seconds-48d161a42311) if `DSPause.delay` is zero.

### Smart Contract Bugs

* `Gnosis Multisig` - a bug in the multisig would be catastrophic as the contract owners would no be able to upgrade the protocol anymore or otherwise switch to a vote based governance model in the future.
* `Governance Aggregator` - the aggregator has very simple logic that allows authorized addresses to set system parameters. Regardless, a bug would allow unauthorized accounts to take control over the system and cause havoc by setting extremely high `stabilityFee`s in the `TaxCollector` or .
* `VoteQuorum` - quorum bugs would potentially lock voters' protocol tokens indefinitely or would allow an attacker to bypass the voting process.
* `DSPause` - an attacker could bypass the `delay` or, if the authorization logic is flawed, propose and execute transactions that were not voted in the quorum or the multisig.

## 4. Governance Minimization

The governance module is meant to be controlled by the community \(with the use of protocol tokens\) or by the core team \(in the initial stages post launch\). Governance minimization is done at the protocol level by removing the multisig, `DSPause` and the `Governance Aggregator` from most contracts'`authorizedAccounts`.

