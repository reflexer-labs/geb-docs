---
description: 'The protocol''s accountant, keeping track of surplus and deficit'
---

# Accounting Engine

## 1. Summary <a id="1-introduction-summary"></a>

The `AccountingEngine` receives both system surplus and system debt. It covers deficits via debt auctions and dispose off surplus via auctions \(`Burning/RecyclingSurplusAuctionHouse`\) or transfers \(to `extraSurplusReceiver`\).

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `contractEnabled` - settlement flag \(`1` or `0`\).
* `authorizedAccounts[usr: address]` - addresses allowed to call `modifyParameters()` and `disableContract()`.
* `safeEngine` - address of the `SAFEEngine`.
* `surplusAuctionHouse` - address of the `PreSettlementSurplusAuctionHouse`.
* `debtAuctionHouse` - address of the `DebtAuctionHouse`.
* `debtQueue[timestamp: uint256]`- the system debt queue. The `LiquidationEngine` adds a new debt block every time it starts a new collateral auction. Each block can be popped out of the list after 

  `popDebtDelay`seconds.

* `activeDebtAuctions[auctionId: uint256]`- amount of debt auctions that aren't yet settled.
* `postSettlementSurplusDrain`- contract meant to auction/dispose off any remaining surplus after the `AccountingEngine` is disabled \(and in case surplus couldn't be settled with bad debt because of a bug\).
* `protocolTokenAuthority` ****- address of ****authority contract that says which addresses are able to mint an burn protocol tokens.
* `totalQueuedDebt`- the total amount of debt in the queue.
* `totalOnAuctionDebt`- the total amount of debt being auctioned in the `DebtAuctionHouse`.
* `popDebtDelay`- length of time for which a debt block must stay in the `debtQueue`.
* `debtAuctionBidSize`- the fixed amount of debt to be covered by a single debt auction.
* `initialDebtAuctionMintedTokens`- the starting amount of protocol tokens offered to cover the auctioned debt.
* `surplusAuctionAmountToSell`- amount of surplus to be sold in a single surplus auction.
* `surplusBuffer`- threshold that must be exceeded before surplus auctions are possible.
* `disableCooldown`- time that must elapse after the `AccountingEngine` is disabled and until it can send all its remaining surplus to the `postSettlementSurplusDrain`. Must be bigger than `GlobalSettlement.shutdownCooldown`.
* `disableTimestamp`- timestamp when the `AccountingEngine` was disabled.

**Modifiers**

* `isAuthorized` ****- checks whether an address is part of `authorizedAddresses` \(and thus can call authed functions\).

**Functions**

* `modifyParameters(bytes32 parameter`, `uint256 data)` - update a `uint256` parameter.
* `modifyParameters(bytes32 parameter`, `address data)` - update an `address` parameter.
* `addAuthorization(usr: address)` - add an address to `authorizedAddresses`.
* `removeAuthorization(usr: address)` - remove an address from `authorizedAddresses`.
* `pushDebtToQueue(debtBlock: uint256)` - adds a bad debt block to the auctions queue.
* `popDebtFromQueue(timestamp: uint256)` - release a debt block from the debt queue.
* `settleDebt(rad: uint256)` - calls `settleDebt` on the `safeEngine` in order to cancel out surplus and debt.
* `cancelAuctionedDebtWithSurplus(rad: uint256)` - cancels out surplus coming from `DebtAuctionHouse` auctions and auctioned \(bad\) debt.
* `auctionSurplus()` - trigger a surplus auction \(`SurplusAuctionHouse.startAuction`\).
* `auctionDebt()` - trigger a deficit auction \(`DebtAuctionHouse.startAuction`\).
* `settleDebtAuction(id: uint256)` - authed function meant to be called by `debtAuctionHouse` in order to signal that a specific auction settled.
* `transferPostSettlementSurplus()` - transfer any post settlement, leftover surplus to the`postSettlementSurplusDrain`. Meant to be a backup in case `GlobalSettlement.processSAFE` has a bug \(cannot process a specific Safe\), governance doesn't have power over the system and there's still surplus left in the `AccountingEngine` which then blocks `GlobalSettlement.setOutstandingCoinSupply`.
* `disableContract()` - set `contractEnabled` to zero and settle as much remaining debt as possible \(if any\)

**Events**

* `AddAuthorization` - emitted when a new address becomes authorized. Contains:
  * `account` - the new authorized account
* `RemoveAuthorization` - emitted when an address is de-authorized. Contains:
  * account - the address that was de-authorized
* `ModifyParameters` - emitted when a parameter is modified
* `PushDebtToQueue` - emitted when a new debt block is added to `debtQueue`. Contains:
  * `timestamp` - timestamp at which the debt block is pushed into `debtQueue`
  * `debtQueueBlock` - the size of the debt block
  * `totalQueuedDebt` - total amount of queued debt in the queue
* `PopDebtFromQueue` - emitted when a debt block is popped out of the `debtQueue`. Contains:
  * `timestamp` - timestamp from which all the debt is popped out of the queue
  * `debtQueueBlock` - amount of debt popped from the queue
  * `totalQueuedDebt` - total remaining amount of queued debt in the queue
* `SettleDebt` - emitted when an amount of debt that is not queued or in a debt auction is settled with an equal amount of surplus. Contains:
  * `rad` - the amount of debt to settle
  * `coinBalance` - the remaining amount of surplus after the debt is settled
  * `debtBalance` - the remaining amount of debt after `rad` debt is settled
* `CancelAuctionedDebtWithSurplus` - emitted when the contract settles debt that was in a debt auction. Contains:
  * `rad` - amount of auctioned debt to settle
  * `totalOnAuctionDebt` - remaining amount of debt being auctioned
  * `coinBalance` - the `AccountingEngine`'s coin balance after the debt is settled
  * `debtBalance` - remaining amount of debt 
* `AuctionDebt` - emitted when a new debt auction starts. Contains:
  * `id` - the ID of the new debt auction
  * `totalOnAuctionDebt` - total debt being auctioned across all debt auctions
  * `debtBalance` - the `AccountingEngine`'s debt balance in the `SAFEEngine`
* `AuctionSurplus` - emitted when a new surplus auction starts. Contains:
  * `id` - the ID of the new surplus auction
  * `lastSurplusAuctionTime` - the current timestamp which is now the last time when a surplus auction was triggered
  * `coinBalance` - the `AccountingEngine`'s surplus balance
* `DisableContract` - emitted when the contract is disabled
* `TransferPostSettlementSurplus` - emitted when any remaining surplus after the contract is disabled is transferred to the `postSettlementSurplusDrain.` Contains:
  * `postSettlementSurplusDrain` - the address of the surplus drain
  * `coinBalance` - the remaining surplus balance of the `AccountingEngine`
  * `debtBalance` - the remaining debt balance of the `AccountingEngine`

## 3. Walkthrough

### Auctioning Debt

When a SAFE is liquidated, the seized debt is put in a queue in the `AccountingEngine`. This occurs at the block timestamp of the `liquidateSAFE` action \(`debtQueue[timestamp]`\). It can be released with the help of `popDebtFromQueue` once `AccountingEngine.popDebtDelay` has expired. Once released, it can be settled using the surplus gathered from the SAFE's liquidation or, if there wasn't enough surplus gathered, the debt can be auctioned using the `DebtAuctionHouse`. The main risk is related to `popDebtDelay` &lt; `CollateralAuctionHouse.totalAuctionLength` which would result in debt auctions starting before the associated collateral auctions could complete.

### Auctioning Surplus

When the `AccountingEngine` has a surplus balance above the `surplusBuffer` \(`safeEngine.coinBalance[accountingEngine]` &gt; `surplusBuffer`\) and if the extra surplus on top of the buffer is not reserved to nullify the engine's bad debt \(`safeEngine.debtBalance[accountingEngine]`\) it can be auctioned off using the `PreSettlementSurplusAuctionHouse`. This process results in burning protocol tokens that are being offered in exchange for the auctioned surplus.

### Disabling the Accounting Engine

When an authorized address calls `AccountingEngine.disableContract` the system will try to settle as much remaining `safeEngine.debtBalance[accountingEngine]` as possible. 

