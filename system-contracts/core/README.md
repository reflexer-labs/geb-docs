---
description: 'Handling SAFE state, liquidations and accounting (surplus & bad debt)'
---

# Core Module

**Relevant smart contracts:**

* \*\*\*\*[**SAFEEngine**](https://github.com/reflexer-labs/geb/blob/master/src/SAFEEngine.sol)\*\*\*\*
* \*\*\*\*[**LiquidationEngine**](https://github.com/reflexer-labs/geb/blob/master/src/LiquidationEngine.sol)\*\*\*\*
* \*\*\*\*[**AccountingEngine**](https://github.com/reflexer-labs/geb/blob/master/src/AccountingEngine.sol)\*\*\*\*

## 1. Overview

The **Core Module** stores all the SAFE data, allows external actors to trigger liquidations in case SAFEs are underwater and also handles debt and surplus auctions.

## 2. Component Descriptions

* The `SAFEEngine`stores all SAFEs' states and system coin balances, as well as the amount of collateral and debt each address has. This contract is self-contained and has no external dependencies.
* The `LiquidationEngine` is meant to check if a SAFE is unsafe \(the value of the issued debt is too high compared to the collateral value\) and start a collateral auction that sells a portion of the SAFE's collateral in order to cover a share of its debt.
* The `AccountingEngine` stores the overall system surplus and debt data. It is meant to settle deficit via debt auctions and dispose off surplus via surplus auctions or basic transfers.

## 3. Risks

### Smart Contract Bugs <a id="coding-errors"></a>

* `SAFEEngine` - A bug in the `SAFEEngine` could be fatal and would lead to collateral or debt being stuck in the system
* `LiquidationEngine` - A bug in the `LiquidationEngine` could lead debt or collateral being assigned to addresses from where they cannot be recovered. Compared to MCD, the `LiquidationEngine` can call external contracts that are meant to save SAFEs by adding more collateral in the system. These "insurance" contracts, if coded incorrectly, can change system state without actually adding any collateral and thus block the engine from starting new auctions. The `liquidateSAFE(bytes32 collateralType, address cdp)` function also uses mutexes to prevent re-entrancy. If a mutex is not unassigned at the end of the call, it can prevent the `LiquidationEngine` from liquidating a specific SAFE in the future.
* `AccountingEngine` - A bug in the `AccountingEngine` would prevent the system from reaching equilibrium \(by auctioning debt or disposing off surplus\).

### Price Feeds <a id="feeds"></a>

Both the `SAFEEngine` and the `LiquidationEngine` rely \(directly or indirectly\) on the `OracleRelayer` which in turn receives price data from multiple trusted sources. If the price feed oracles fail, it's possible that SAFEs will be unfairly liquidated or that users will generate unbacked debt.

### All-Powerful Governance <a id="governance"></a>

* `SAFEEngine` - Malicious governance can steal collateral \(`modifyCollateralBalance`\) or mint unbacked debt for no apparent reason \(`createUnbackedDebt`/addition of worthless collateral types\).
* `LiquidationEngine` - Governance could misconfigure liquidation parameters \(e.g an extremely low or high `liquidationPenalty`\).
* `AccountingEngine` - Malicious governance can set null addresses as the `debtAuctionHouse` or the `surplusAuctionHouse` and thus not allow the system to reach equilibrium or even trigger settlement. The can also set a faulty `AccountingEngine.systemStakingPool` which can prevent the engine from starting new debt auctions and thus leave deficit in the system

## 4. Governance Minimization

* `SAFEEngine` \(Level 2 Gov Minimization\) - the `SAFEEngine` will need an external contract to automatically set the `globalDebtCeiling` and each collateral's individual `debtCeiling`s. Apart from this, governance can remove control from the `SAFEEngine`.
* `LiquidationEngine` \(Level 1 Gov Minimization\) - the `LiquidationEngine` may have an external contract authorized to periodically set `onAuctionSystemCoinLimit` depending on the current outstanding amount of system coins generated. Governance will also need to have control over connecting and disconnecting saviour contracts because they are external dependencies connected to other protocols/3rd parties. Apart from these, governance can remove control over this contract.
* `AccountingEngine` \(Level 2 Gov Minimization\) - the `AccountingEngine` must authorize an external contract to automatically set `initialDebtAuctionMintedTokens` and `debtAuctionBidSize` according to the protocol token and system coin market prices. Governance may also keep control over setting `systemStakingPool` in case the staking pool still needs to be upgraded and may cause problems in the engine.

