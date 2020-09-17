---
description: Tools for achieving consensus
---

# Governance Module

**Relevant smart contracts:**

* [DSPause](https://github.com/reflexer-labs/ds-pause/blob/master/src/pause.sol)

## 1. Overview

The **Governance Module** is a set of smart contracts that governance can use to modify and upgrade GEB.

## 2. Component Descriptions

* `DSPause` - the pause contract enforces a delay between scheduling a transaction \(coming from a multisig or a voting contract\) and executing it.
* `DSProtestPause` - this pause contract inherits the same functionality from `DSPause` and it also allows a `protester` to further delay any transaction that they do not agree with. The protest mechanism is an experiment meant to allow token holders to delay any governance attack during the system's initial stages.

## 3. Risks

### Governance Attack

Malicious governance may want to extract all the collateral from the system or generate a high amount of debt that is not backed by collateral. There are two possible solutions to this problem:

1. Eliminate governance over most core system components \(especially the ones that handle collateral balances\).
2. Use `DSPause` or `DSProtestPause` and add a delay to every schedule governance proposal.

### Smart Contract Bugs

* `DSPause` / `DSProtestPause` - an attacker could bypass the `delay` or, if the authorization logic is flawed, propose and execute transactions that were not approved by token holders or by a multisig.

## 4. Governance Minimization

The governance module is meant to be controlled by the community \(with the use of protocol tokens\) or by the core team \(in the initial stages post launch\). Governance minimization is done at the protocol level by removing or bounding human control.

