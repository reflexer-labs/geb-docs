---
description: 'Handling CDP state, liquidations and accounting (surplus & bad debt)'
---

# Core Module

**Relevant smart contracts:**

* \*\*\*\*[**CDPEngine**](https://github.com/reflexer-labs/geb/blob/master/src/CDPEngine.sol)\*\*\*\*
* \*\*\*\*[**LiquidationEngine**](https://github.com/reflexer-labs/geb/blob/master/src/LiquidationEngine.sol)\*\*\*\*
* \*\*\*\*[**AccountingEngine**](https://github.com/reflexer-labs/geb/blob/master/src/AccountingEngine.sol)\*\*\*\*

## 1. Overview

The **Core Module** stores all the CDP data, allows external actors to trigger liquidations in case CDPs are underwater and also handles debt and surplus auctions.

## 2. Component Descriptions

* The `CDPEngine`stores all CDPs' states and system coin balances, as well as the amount of collateral debt each address has. This contract is self-contained and has no external dependencies.
* The `LiquidationEngine` is meant to check if a CDP is unsafe \(the value of the issued debt is too high compared to the collateral value\) and start a collateral auction that sells a portion of the CDP's collateral in order to cover a share of its debt.
* The `AccountingEngine` stores the overall system surplus and debt data. It is meant to settle deficit via debt auctions and sell off surplus via surplus auctions.

## 3. Risks

### Smart Contract Bugs <a id="coding-errors"></a>

* `CDPEngine` - A bug in the `CDPEngine` could be fatal and would lead to collateral or debt being stuck
* `LiquidationEngine` - A bug in the `LiquidationEngine` could lead debt or collateral being assigned to addresses from where they cannot be recovered. Compared to MCD, the `LiquidationEngine` can call external contracts that are meant to save CDPs by adding more collateral in the system. These "insurance" contracts, if coded incorrectly, can change system state without actually adding any collateral and thus block the engine from starting new auctions. The `liquidateCDP(bytes32 collateralType, address cdp)` function also uses mutexes to prevent re-entrancy. If a mutex is not unassigned at the end of the call, it can prevent the `LiquidationEngine` from liquidating a specific CDP in the future.
* `AccountingEngine` - A bug in the `AccountingEngine` would prevent the system from reaching equilibrium \(by auctioning debt or surplus\). Compared to MCD, the `AccountingEngine` keeps track of currently active debt auctions and also transfers remaining surplus \(after settling as much debt as possible\) in a separate auctioneer contract in case an authorized address calls `disableContract()`. If the receiver of leftover surplus is set incorrectly, the funds may be lost forever.

### Price Feeds <a id="feeds"></a>

Both the `CDPEngine` and the `LiquidationEngine` rely \(directly or indirectly\) on the `OracleRelayer` which in turn receives price data from multiple trusted sources. If the price feed oracles fail, it's possible that CDPs will be unfairly liquidated or that users will generate unbacked debt.

### All-Powerful Governance <a id="governance"></a>

* `CDPEngine` - Malicious governance can steal collateral \(`modifyCollateralBalance`\) or mint unbacked debt for no apparent reason \(`createUnbackedDebt`/addition of worthless collateral types\).
* `LiquidationEngine` - Governance could misconfigure liquidation parameters \(e.g an extremely low or high `liquidationPenalty`\).
* `AccountingEngine` - Malicious governance can set null addresses as the `debtAuctionHouse` or the `surplusAuctionHouse` and thus not allow the system to reach equilibrium or even trigger settlement. They can set a null address as the `settlementSurplusAuctioner` and make any extra surplus \(post settlement\) unretrievable.

## 4. Governance Minimization

* `CDPEngine` - governance can keep control over raising \(never lowering\) the `globalDebtCeiling` as well as individual collateral `debtCeilings`, especially when the system is backed only by one collateral type and the [automated feedback mechanism](https://docs.reflexer.finance/system-contracts/feedback-mechanism-module) takes care of changing user incentives.
* `LiquidationEngine` - the `liquidationPenalty` may be changed within certain bounds. There can be lower and upper limits on the value the penalty can be set to or governance can be restricted so they can only change the penalty X% every Y seconds.
* `AccountingEngine` - governance can completely remove control over this contract. Parameters such as the `surplusBuffer` can be automatically set by other autonomous smart contracts.

