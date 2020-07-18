---
description: 'The protocol''s accountant, keeping track of surplus and deficit'
---

# Accounting Engine

## 1. Summary <a id="1-introduction-summary"></a>

The `AccountingEngine` receives both system surplus and system debt. It covers deficits via debt auctions and sell off surplus via surplus \(`Pre/PostSettlementSurplusAuctionHouse`\) auctions.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `contractEnabled` - settlement flag \(`1` or `0`\).
* `authorizedAccounts[usr: address]` - addresses allowed to call `modifyParameters()` and `disableContract()`.
* `cdpEngine` - address of the `CDPEngine`.
* `surplusAuctionHouse` - address of the `PreSettlementSurplusAuctionHouse`.
* `debtAuctionHouse` - address of the `DebtAuctionHouse`.
* `debtQueue[timestamp: uint256]`- the system debt queue. The `LiquidationEngine` adds a new debt block every time it starts a new collateral auction. Each block can be popped out of the list after 

  `popDebtDelay`seconds.

* `activeDebtAuctions[auctionId: uint256]`- amount of debt auctions that aren't yet settled.
* `postSettlementSurplusDrain`- contract meant to auction/dispose off any remaining surplus after the `AccountingEngine` is disabled.
* `protocolTokenAuthority` ****- address of ****authority contract that says which addresses are able to mint an burn protocol tokens.
* `totalQueuedDebt`- the total amount of debt in the queue.
* `totalOnAuctionDebt`- the total amount of debt being auctioned in the `DebtAuctionHouse`.
* `popDebtDelay`- length of time for which a debt block must stay in the `debtQueue`.
* `debtAuctionBidSize`- the fixed amount of debt to be covered by a single debt auction.
* `initialDebtAuctionMintedTokens`- the starting amount of protocol tokens offered to cover the auctioned debt.
* `surplusAuctionAmountToSell`- amount of surplus to be sold in a single surplus auction.
* `surplusBuffer`- threshold that must be exceeded before surplus auctions are possible.
* `disableCooldown`- time that must elapse after the `AccountingEngine` is disabled and until it can send all its remaining surplus to the `postSettlementSurplusDrain`.
* `disableTimestamp`- timestamp when the `AccountingEngine` was disabled.

**Modifiers**

* `isAuthorized` ****- checks whether an address is part of `authorizedAddresses` \(and thus can call authed functions\).

**Functions**

* \`\`
* `pushDebtToQueue` - adds a bad debt block to the auctions queue.
* `popDebtFromQueue` - release a debt block from the debt queue.
* `settleDebt` - calls `settleDebt` on the `cdpEngine` in order to cancel out surplus and debt. 
* `cancelAuctionedDebtWithSurplus` - cancels out surplus coming from `DebtAuctionHouse` auctions and auctioned \(bad\) debt.
* `auctionSurplus` - trigger a surplus auction \(`SurplusAuctionHouse.startAuction`\).
* `auctionDebt` - trigger a deficit auction \(`DebtAuctionHouse.startAuction`\).
* settleDebtAuction -
* `transferPostSettlementSurplus` - transfer any post settlement, leftover surplus to the`postSettlementSurplusDrain.` Throws if `disableCooldown` haven't yet passed since `disableTimestamp`
* disableContract -

## 3. Walkthrough

### Auctioning Debt

When a CDP is liquidated, the seized debt is put in a queue in the `AccountingEngine`. This occurs at the block timestamp of the `liquidateCDP` action \(`debtQueue[timestamp]`\). It can be released with the help of `popDebtFromQueue` once `AccountingEngine.popDebtDelay` has expired. Once released, it can be settled using the surplus gathered from the CDP's liquidation or, if there was not enough surplus gathered, the debt can be auctioned using the `DebtAuctionHouse`. The main risk is related to `popDebtDelay` &lt; `CollateralAuctionHouse.totalAuctionLength` which would result in debt auctions starting before the associated collateral auctions could complete.

### Auctioning Surplus

When the AccountingEngine has a surplus balance above the `surplusBuffer` \(`cdpEngine.coinBalance[accountingEngine]` &gt; `surplusBuffer`\) and if the extra surplus on top of the buffer is not reserved to nullify the engine's bad debt \(`cdpEngine.debtBalance[accountingEngine]`\) it can be auctioned off using the `PreSettlementSurplusAuctionHouse`. This process results in burning protocol tokens that are being bid in exchange for surplus.

### Disabling the Accounting Engine

When an authorized address calls `AccountingEngine.disableContract` the system will try to settle as much remaining `cdpEngine.debtBalance[accountingEngine]` as possible before sending any remaining surplus to the `postSettlementSurplusDrain`\(right away if `disableCooldown` is zero or after `disableCooldown` seconds have passed since `disableTimestamp`\).

