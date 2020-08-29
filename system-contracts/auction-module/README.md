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

* The `CollateralAuctionHouse` is used to sell collateral from SAFEs that have become under-collateralized in order to preserve the overall health of the system. There are two flavours of collateral auctions: `English` and `FixedDiscount`. 
  * The `English` auction version requires that bidders compete with increasing amounts of system coins for a fixed amount of collateral and only one bidder can win. This auction type has two phases: `increaseBidSize` where bidders submit higher system coin bids and `decreaseSoldAmount` where bidders accept a lower collateral amount for the winning system coin bid.
  * On the other hand, the `FixedDiscount` auction only has one phase \(`buyCollateral`\) where bidders submit system coins and the smart contract offers them collateral at a discounted price compared to its price. This auction type is more capital efficient and user friendly compared to `English` auctions. It also allows anyone to buy collateral using [flashloans](https://blog.coincodecap.com/what-are-flash-loans-on-ethereum).

    By default, the contract will use the collateral's `OSM` price \(which will lag compared to the actual collateral market price\) and the system coin's `redemptionPrice` in order to calculate the amount of collateral to offer in exchange for each individual bid. On the other hand, governance can set the contract's parameters so that it uses the collateral's medianizer price and/or the system coin's market price if they deviated within certain limits from the `OSM` and the `redemptionPrice`. Read more about the formulas here.
* The `DebtAuctionHouse` is used to get rid of the `AccountingEngine`’s debt by auctioning off protocol tokens for a fixed amount of surplus \(system coins\). After the auction is settled, it sends the received surplus to the `AccountingEngine` in order to cancel out bad debt and it also mints protocol tokens for the winning bidder.
* The `SurplusAuctionHouse` \(both its pre and post settlement versions\) is used to get rid of the `AccountingEngine`’s surplus by auctioning off a fixed amount of internal system coins in exchange for protocol tokens. After auction settlement, the auction house burns the winning protocol token bid and sends internal system coins to the winning bidder. There are two flavors of surplus auctions: the first one used while the system is not settled and the other one used to auction leftover surplus after the system settles.
* The `SettlementSurplusAuctioneer` is \(currently\) in charge with disbursing remaining surplus after `GlobalSettlement` is triggered and the `AccountingEngine` covers as much of its remaining debt as possible before being disabled.

## 3. Risks <a id="5-failure-modes-bounds-on-operating-conditions-and-external-risk-factors"></a>

Governance needs to fine-tune auction parameters in order to make the bidding process as efficient as possible. In the case of `English`, `Debt` and `Surplus` auctions there are three main parameters:

* `bidIncrease`: if it's too high it will discourage bidding and if it's too low its effect will be insignificant
* `bidDuration`: if it's too high it would force the winning bidder to wait too long until they can collect their winnings. If it's too low it would not give other bidders enough time to participate
* `totalAuctionLength`: if it's too high it would only delay auction settlement without any positive impact on the outcome. If it's too low it would make the auction settle before the true price is found.
* `bidToMarketPriceRatio` \(only in `English` collateral auctions\): minimum mandatory size of the first bid compared to collateral price coming from the `OSM`

And in the case of `FixedDiscount` collateral auctions there's a wider range of variables:

* `totalAuctionLength`: same parameter as in the other auction types
* `discount`: discount applied to the collateral price
* `minimumBid`: minimum system coin bid that must be submitted by any bidder
* `upperMedianDeviation`: maximum upper side medianizer price deviation \(compared to the `OSM` price\) used by the auction contract to determine whether it will use the median or the `OSM` as a feed source
* `lowerMedianDeviation`: maximum lower side medianizer price deviation \(compared to the `OSM` price\) used by the auction contract to determine whether it will use the median or the `OSM` as a feed source

## 4. Governance Minimization

Governance can completely remove control over all auction contracts.

