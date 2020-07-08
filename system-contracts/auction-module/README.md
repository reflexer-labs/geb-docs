---
description: Maintaining system balance by covering shortfall and disbursing surplus
---

# Auction Module

**Relevant smart contracts:**

* \*\*\*\*[**CollateralAuctionHouse**](https://github.com/reflexer-labs/geb/blob/master/src/CollateralAuctionHouse.sol)\*\*\*\*
* \*\*\*\*[**DebtAuctionHouse**](https://github.com/reflexer-labs/geb/blob/master/src/DebtAuctionHouse.sol)\*\*\*\*
* \*\*\*\*[**SurplusAuctionHouse**](https://github.com/reflexer-labs/geb/blob/master/src/SurplusAuctionHouse.sol)\*\*\*\*
* \*\*\*\*[**SettlementSurplusAuctioneer**](https://github.com/reflexer-labs/geb/blob/master/src/SettlementSurplusAuctioneer.sol)\*\*\*\*

## 1. Overview

The **Auction Module** is meant to incentivize external actors to drive the system back to a safe state by participating in collateral, debt and surplus auctions.

## 2. Component Descriptions

* The `CollateralAuctionHouse` is used to sell collateral from CDPs that have become under-collateralized in order to preserve the overall health of the system. The collateral auction has two phases: `increaseBidSize` and `decreaseSoldAmount`.
* The `DebtAuctionHouse` is used to get rid of the `AccountingEngine`’s debt by auctioning off protocol tokens for a fixed amount of surplus \(system coins\). After the auction is settled, it sends the received surplus to the `AccountingEngine` in order to cancel out bad debt and it also mints protocol tokens for the winning bidder.
* The `SurplusAuctionHouse` \(both its pre and post settlement versions\) is used to get rid of the `AccountingEngine`’s surplus by auctioning off a fixed amount of internal system coins in exchange for protocol tokens. After auction settlement, the auction house burns the winning protocol token bid and sends internal system coins to the winning bidder. There are two flavors of surplus auctions: the first one used while the system is not settled and the other one used to auction leftover surplus after the system settles.
* The `SettlementSurplusAuctioneer` is \(currently\) in charge with disbursing remaining surplus after `GlobalSettlement` is triggered and the `AccountingEngine` covers as much of its remaining debt as possible before being disabled.

## 3. Risks <a id="5-failure-modes-bounds-on-operating-conditions-and-external-risk-factors"></a>

Governance needs to fine-tune auction parameters in order to make the bidding process as efficient as possible.

* `bidIncrease`: if it's too high it will discourage bidding and if it's too low its effect will be insignificant
* `bidDuration`: if it's too high it would force the winning bidder to wait too long until they can collect their winnings. If it's too low it would not give other bidders enough time to participate
* `totalAuctionLength`: if it's too high it would only delay auction settlement without any positive impact on the outcome. If it's too low it would make the auction settle before the true price is found. 

## 4. Governance Minimization

Governance can completely remove control over all auction contracts.

