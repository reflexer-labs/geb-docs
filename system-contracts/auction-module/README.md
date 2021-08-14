---
description: Maintaining system balance by covering shortfall and disbursing surplus
---

# Auction Module

**Relevant smart contracts:**

* \*\*\*\*[**CollateralAuctionHouse**](https://github.com/reflexer-labs/geb/blob/master/src/single/CollateralAuctionHouse.sol)\*\*\*\*
* \*\*\*\*[**DebtAuctionHouse**](https://github.com/reflexer-labs/geb/blob/master/src/single/DebtAuctionHouse.sol)\*\*\*\*
* \*\*\*\*[**SurplusAuctionHouse**](https://github.com/reflexer-labs/geb/blob/master/src/single/SurplusAuctionHouse.sol)\*\*\*\*

## 1. Overview

The **Auction Module** is meant to incentivize external actors to drive the system back to a safe state by participating in collateral, debt and surplus auctions.

## 2. Component Descriptions

* The `CollateralAuctionHouse` is used to sell collateral from SAFEs that have become under-collateralized in order to preserve the overall health of the system. There are two flavours of collateral auctions: `English` and `FixedDiscount`. 
  * The `English` auction version requires that bidders compete with increasing amounts of system coins for a fixed amount of collateral and only one bidder can win. This auction type has two phases: `increaseBidSize` where bidders submit higher system coin bids and `decreaseSoldAmount` where bidders accept a lower collateral amount for the winning system coin bid.
  * On the other hand, the `FixedDiscount` and `IncreasingDiscount` auctions only have one phase \(`buyCollateral`\) where bidders submit system coins and the smart contract offers them collateral at a discounted price compared to its market price. These auction types are more capital efficient and user friendly compared to `English` auctions. They also allow anyone to buy collateral using [flashloans](https://blog.coincodecap.com/what-are-flash-loans-on-ethereum).

    By default, the contract will use the collateral's `OSM` price \(which will lag compared to the actual collateral market price\) and the system coin's `redemptionPrice` in order to calculate the amount of collateral to offer in exchange for each individual bid. There is the possibility for governance to set the contract's parameters so that it uses the collateral's medianizer price and/or the system coin's market price if they deviated within certain limits from the `FSM` price and the `redemptionPrice`.
* The `DebtAuctionHouse` is used to get rid of the `AccountingEngine`’s debt by auctioning off protocol tokens for a fixed amount of surplus \(system coins\). After the auction is settled, it sends the received surplus to the `AccountingEngine` in order to cancel out bad debt and it also mints protocol tokens for the winning bidder.
* The `SurplusAuctionHouse` \(all of its `Burning`, `Recycling` and `PostSettlement` versions\) is used to get rid of the `AccountingEngine`’s surplus by auctioning off a fixed amount of internal system coins in exchange for protocol tokens. After auction settlement, the auction house either burns the winning protocol token bid or it transfers the bid to an external address and then sends internal system coins to the winning bidder.

## 3. Risks <a id="5-failure-modes-bounds-on-operating-conditions-and-external-risk-factors"></a>

Governance needs to fine-tune auction parameters in order to make the bidding process as efficient as possible. In the case of `English`, `Debt` and `Surplus` auctions there are three main parameters:

* `bidIncrease`- if it's too high it will discourage bidding and if it's too low its effect will be insignificant
* `bidDuration`- if it's too high it would force the winning bidder to wait too long until they can collect their winnings. If it's too low it would not give other bidders enough time to participate
* `totalAuctionLength`- if it's too high it would only delay auction settlement without any positive impact on the outcome. If it's too low it would make the auction settle before the true price is found.
* `bidToMarketPriceRatio` \(only in `English` collateral auctions\) - minimum mandatory size of the first bid compared to collateral price coming from the `OSM`

In the case of `FixedDiscount` collateral auctions there's a wider range of variables:

* `totalAuctionLength`- same parameter as in the other auction types
* `discount`- discount applied to the collateral price
* `minimumBid`- minimum system coin bid that must be submitted by any bidder
* `upperCollateralMedianDeviation`- maximum upper side medianizer price deviation \(compared to the `OSM` price\) used by the auction contract to determine whether it will use the median or the `OSM` as a feed source
* `lowerCollateralMedianDeviation`- maximum lower side medianizer price deviation \(compared to the `OSM` price\) used by the auction contract to determine whether it will use the median or the `OSM` as a feed source
* `lowerSystemCoinMedianDeviation` - maximum lower side system coin market price \(compared to the redemption price\) used by the auction contract to determine whether it will use the market or the redemption price when determining the amount of collateral bought
* `upperSystemCoinMedianDeviation` - maximum upper side system coin market price \(compared to the redemption price\) used by the auction contract to determine whether it will use the market or the redemption price when determining the amount of collateral bought 
* `minSystemCoinMedianDeviation` - minimum deviation between the market and the redemption prices of the system coin in order for the contract to choose the market price \(and not the redemption one\) when it determines the amount of collateral bought

In the case of `IncreasingDiscount` collateral auctions, the parameter range is almost identical to the one in `FixedDiscount` auctions, with some additions:

* `minDiscount` - minimum discount \(compared to the system coin's current redemption price\) at which collateral is being sold
* `maxDiscount` - maximum discount \(compared to the system coin's current redemption price\) at which collateral is being sold
* `perSecondDiscountUpdateRate` - rate at which the discount will be updated in an auction
* `maxDiscountUpdateRateTimeline` - max time over which the discount can be updated in an auction

## 4. Governance Minimization

All of the auction contracts are in the Level 1 Gov Minimization category.

