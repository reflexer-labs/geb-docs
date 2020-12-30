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

Each component in GEB has varying degrees of governance minimization potential. The following is a \(incomplete\) list of contracts and how much each one can be gov minimized:

* **Accounting Engine** - governance may need to keep control over setting `systemStakingPool` until the pool is governance minimized; `initialDebtAuctionMintedTokens` and `debtAuctionBidSize` will need to be set by an external contract
* **Collateral Token Adapters** - governance can completely remove control from collateral adapters
* **Coin** - governance can completely remove control from the system coin ERC20 contract
* **Collateral Auction House** - 
* **Debt Auction House** -
* **Surplus Auction House** -
* **Global Settlement** -
* **ESM** -
* **Liquidation Engine** -
* **Oracle Relayer** -
* **SAFE Engine** -
* **Stability Fee Treasury** -
* **Tax Collector** -
* **OSMs/DSMs** - 
* **Medianizers** -
* **FSM Governance Interface** -
* **DSPause** -
* **PID Controller** -
* **Saviour Contracts** -
* **SAFE Saviour Registry** -

### 3. Infrastructure for Governance Minimization

### 3. Governance Minimization Levels

