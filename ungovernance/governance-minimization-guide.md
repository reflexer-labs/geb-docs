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
* The system must accrue enough surplus in its main treasury so that it affords to pay for oracle, PID, state management etc costs for at least 6 months

### 2. How Much Can GEB Be Governance Minimized

Each component in GEB has varying degrees of governance minimization potential. The following is a \(incomplete\) list of contracts and which parameters will still need to be managed after governance minimization:

* **Accounting Engine** - governance may need to keep control over setting `systemStakingPool` until the pool is governance minimized; `initialDebtAuctionMintedTokens` and `debtAuctionBidSize` will need to be set by an external contract which will be connected to oracles \(thus this external contract will not be fully gov minimized\); an optional contract may set `surplusBuffer` so that it covers a specific percentage of the outstanding supply of system coins minus the surplus from the Accounting Engine and the one from the Stability Fee Treasury/ies; another external contract should reward addresses that call `popDebtFromQueue`
* **Collateral Token Adapters** - governance can completely remove control from these contracts
* **Coin** - governance can completely remove control from this contract
* **Collateral Auction House** - in the current fixed discount implementation, governance will need to keep control over setting `systemCoinOracle`; the rest of the contract can be governance minimized
* **Debt Auction House** - governance can completely remove control from this contract
* **Surplus Auction House** - governance can completely remove control from this contract
* **Global Settlement** - once all the other core contracts are governance minimized, governance can remove control from this contract
* **ESM** - governance will need to set an external contract `thresholdSetter` that recomputes `triggerThreshold` according to the latest outstanding supply of protocol tokens; this can only be done once; the rest of the contract can be governance minimized
* **Liquidation Engine** - governance will keep control over connecting and disconnecting saviours; governance may optionally authorize an external contract to automatically set`onAuctionSystemCoinLimit`
* **Oracle Relayer** - governance may be allowed to set the redemption rate to 0% only; apart from this governance can completely be removed
* **SAFE Engine** - governance will need to allow an external contract to automatically set `debtCeiling`s for every collateral type once every couple of hours/days; governance should also allow a contract to adjust `debtFloor`s according to the latest redemption price; depending on how many collateral types are in a system, it may not be feasible to automatically set debt ceilings but rather manually vote on lowering/raising them
* **Stability Fee Treasury** - governance can keep control over calling `takeFunds` because it does not affect permissions or other contract behavior; governance should also keep control over resetting `total` allowances to their initial values for all accounts that can `pullFunds` post governance minimization
* **Tax Collector** - governance may optionally want to have bounded control over setting stability fees; by bounded we mean that there will be upper and lower bounds for setting each collateral's fee e.g between 1-2% annually; apart from this, the contract can be governance minimized
* **OSMs/DSMs** - governance will need to keep maintaining these components in the long run because they are connected to medianizers which are in turn connected to external components \(oracles\)
* **Medianizers** - governance will need to keep maintaining these components in the long run because they are connected to external components
* **FSM Governance Interface** - governance will need to keep maintaining this component in the long run because it is managing OSMs/DSMs which are not gov minimized
* **DSPause** - this components is part of the governance module and it will be, by definition, governed in the long run
* **Protocol Token Authority** - governance can completely remove control from this contract once the Debt Auction House is governance minimized
* **Protocol Token Printing Permissions** - governance can completely remove control from this contract once the Debt Auction House is governance minimized
* **Protocol Token** - governance will not have control over this component \(manually minting tokens or changing allowances so other addresses can mint\) once they remove control from the Protocol Token Authority and the Protocol Token Printing Permissions
* **PID Controller** - governance may need to keep some control over this component in the long run; the community will have more insight into how much control it will need after a GEB has been running for at least 1 year on mainnet; one reason for maintaining \(bounded\) control is the fact that the controller should be paused when the system's reflex index doesn't have enough liquidity on exchanges; **NOTE**: even if governance keeps some control over the PID, the `OracleRelayer` will have upper and lower bounds for the redemption rate so that a potential governance attack cannot immediately destroy the protocol
* **Saviour Contracts** - governance will need to keep maintaining these contracts in the long run because they are connected to external components
* **SAFE Saviour Registry** - governance will need to keep maintaining this contract in the long run because it's meant to whitelist/blacklist saviour contracts

### 3. Infrastructure for Automation

A couple of GEB contracts will need to authorize other components to automatically set some of their parameters post governance minimization. Here is the current list of external components for every GEB contract:

* **Liquidation Engine** - an optional contract that automatically sets `onAuctionSystemCoinLimit` as a percentage of the current amount of system coins minus the surplus accrued in the Accounting Engine and in the Stability Fee Treasury/ies
* **Accounting Engine** - an optional contract may set `surplusBuffer` so that it covers a specific percentage of the outstanding supply of system coins minus the surplus from the Accounting Engine and the one from the Stability Fee Treasury/ies; a mandatory contract that sets`initialDebtAuctionMintedTokens` and `debtAuctionBidSize` every once in a while according to the protocol token and system coin market prices
* **ESM** - `thresholdSetter` which automatically sets `triggerThreshold` as a percentage of the current outstanding supply of protocol tokens
* **Stability Fee Treasury** - governance may need to create external contracts to reset `total` allowances for addresses that have a `perBlock` allowance &gt; 0 and allow the automatic setting of params such as `minimumFundsRequired`, `pullFundsMinThreshold` etc depending on the latest `redemptionPrice`
* **SAFE Engine** - a contract that periodically adjusts `debtCeiling`s for every collateral type; another contract that adjusts `debtFloor`s according to the latest `redemptionPrice` every couple of weeks; the implementation depends on every GEB's setup \(how many collateral types it has, what percentage of system coins should be covered by each collateral etc\)

Lastly, contracts that pull funds from the **Stability Fee Treasury** may need to have their `baseUpdateCallerReward` and `maxUpdateCallerReward` adjusted periodically depending on the latest `redemptionPrice`.

### 4. Governance Minimization Levels

There are three levels \(or stages\) of governance minimization that a GEB \(like the one for RAI\) will go through:

#### Level 1 - Deadline 14 Months Post Launch

In this stage, governance will remove control from:

* LiquidationEngine
* DebtAuctionHouse
* SurplusAuctionHouse
* Collateral Auction Houses
* OracleRelayer
* Coin \(ERC20\)
* CollateralJoin contracts
* ProtocolTokenAuthority \(and as a result give up on the possibility to authorize/deauthorize new or old DebtAuctionHouses to print tokens\)
* ProtocolTokenPrintingPermissions
* TaxCollector
* ESM

#### Level 2 - Deadline 18 Months Post Launch

In this stage, governance will remove control from:

* SAFEEngine
* AccountingEngine
* StabilityFeeTreasury
* GlobalSettlement

Any auxiliary contracts that automate parameter settings should also by themselves be governance minimized.

#### Level 3

At this point, all remaining governance must be in the hands of the community. The community will judge the feasibility of fully removing control from more contracts \(e.g the PID\).

