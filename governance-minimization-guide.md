---
description: Steps and details for minimizing governance over a GEB deployment
---

# Governance Minimization Guide

GEB governance minimization is a multi stage process at the end of which governance will not control or be able to upgrade most core contracts and many parameters will be set autonomously by other external contracts.

This guide will go over the requirements needed to governance minimize a GEB, infrastructure needed to automate parameter setting as well as the governance minimization stages that RAI \(and possibly other reflex indexes\) will go through.

###  1. Requirements for Governance Minimization

In order for a GEB to be governance minimized, there are several requirements that need to be met:

* The protocol's governance must not add or plan to add any more collateral types
* All the infrastructure for governance minimization must have been audited and tested in production
* Governance does not plan to make any more upgrades to the system
* The system must accrue enough surplus in its main treasury so that it affords to pay for oracle, PID, state management etc costs for at least 6 months

### 2. How Much Can GEB Be Governance Minimized

Each component in GEB has varying degrees of governance minimization potential. The following is a \(incomplete\) list of contracts and which parameters will still need to be managed after governance minimization:

* **Accounting Engine** - governance may need to keep control over setting `systemStakingPool` until the pool is governance minimized; `initialDebtAuctionMintedTokens` and `debtAuctionBidSize` will need to be set by an external contract which will be connected to oracles \(thus this external contract will not be fully gov minimized\)
* **Collateral Token Adapters** - governance can completely remove control from these components
* **Coin** - governance can completely remove control from this component
* **Collateral Auction House** - in the current fixed discount implementation, governance will need to keep control over setting `systemCoinOracle`; the rest of the contract can be governance minimized
* **Debt Auction House** - governance can completely remove control from this contract
* **Surplus Auction House** - governance can completely remove control from this contract
* **Global Settlement** - once all the other core contracts are governance minimized, governance can remove control from this contract
* **ESM** - governance will need to set an external contract `thresholdSetter` that recomputes `triggerThreshold` according to the latest outstanding supply of protocol tokens; the rest of the contract can be governance minimized
* **Liquidation Engine** - governance will keep control over connecting and disconnecting saviours; governance may optionally authorize an external contract to automatically set`onAuctionSystemCoinLimit`
* **Oracle Relayer** - governance can completely remove control from this contract
* **SAFE Engine** - governance will need to allow an external contract to automatically set `debtCeiling`s for every collateral type once every couple of hours/days; depending on how many collateral types are in a system, it may not be feasible to automatically set debt ceilings but rather manually vote on lowering/raising them
* **Stability Fee Treasury** - governance can keep control over calling `takeFunds` because it does not affect permissions or other contract behavior; governance should also keep control over resetting `total` allowances to their initial values for all accounts that can `pullFunds` post governance minimization
* **Tax Collector** - governance may optionally want to have bounded control over setting stability fees; by bounded we mean that there will be upper and lower bounds for setting each collateral's fee e.g between 1-2% annually; apart from this, the contract can be governance minimized
* **OSMs/DSMs** - governance will need to keep maintaining these components in the long run because they are connected to medianizers which are in turn connected to external components \(oracles\)
* **Medianizers** - governance will need to keep maintaining these components in the long run because they are connected to external components
* **FSM Governance Interface** - governance will need to keep maintaining this component in the long run because it is managing OSMs/DSMs which are not gov minimized
* **DSPause** - this components is part of the governance module and it will be, by definition, governedin the long run
* **Protocol Token Authority** - governance can completely remove control from this contract once the Debt Auction House is governance minimized
* **Protocol Token Printing Permissions** - governance can completely remove control from this contract once the Debt Auction House is governance minimized
* **Protocol Token** - governance will not have control over this component \(manually minting tokens or changing allowances so other addresses can mint\) once they remove control from the Protocol Token Authority and the Protocol Token Printing Permissions
* **PID Controller** - governance may need to keep some control over this component in the long run; the core team and the community will have more insight into how much control it will need after a GEB has been running for a long time \(at least 1 year\) on mainnet; one reason for maintaining \(bounded\) control is the fact that the controller should be paused when the system's reflex index doesn't have enough liquidity on exchanges; **NOTE**: even if governance keeps some control over the PID, the Oracle Relayer will have upper and lower bounds for the redemption rate so that a potential governance attack cannot immediately destroy the protocol
* **Saviour Contracts** - governance will need to keep maintaining this component in the long run because it is connected to external components
* **SAFE Saviour Registry** - governance will need to keep maintaining this component in the long run because it's meant to whitelist/blacklist saviour contracts

### 3. Infrastructure for Governance Minimization

A couple of GEB contracts will need to authorize other components to automatically set some of their parameters post governance minimization. Here is the current list of external components for every GEB contract:

* **Liquidation Engine** - an optional contract that automatically sets `onAuctionSystemCoinLimit` as a percentage of the current amount of system coins minus the surplus accrued in the `AccountingEngine` and in the `StabilityFeeTreasury`/ies
* **Accounting Engine** - 
* **ESM** -
* **SAFE Engine** -

### 3. Governance Minimization Levels

