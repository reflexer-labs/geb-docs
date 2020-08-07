---
description: >-
  Auctioneer that tries to recapitalize the system by selling collateral at a
  discounted price
---

# Fixed Discount Collateral Auction House

## 1. Summary <a id="1-introduction-summary"></a>

Fixed discount collateral auctions are similar to their `English` counterpart in that they are used to preserve the overall system health by liquidating undercollateralized CDPs and selling off collateral in exchange for system coins. This auction type automatically calculates an amount of collateral to send back to a bidder taking into account to the amount of system coins the bidder submits as well as to the current system coin `redemptionPrice` and collateral market price.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `contractEnabled` - settlement flag \(`1` or `0`\).
* `AUCTION_HOUSE_TYPE` - flag set to `bytes32("COLLATERAL")`
* `AUCTION_TYPE` - flag set to `bytes32("FIXED_DISCOUNT")`.
* `authorizedAccounts[usr: address]` - addresses allowed to call `modifyParameters()` and `disableContract()`.
* `cdpEngine` - storage of the `CDPEngine`'s address.
* `bids[id: uint]` - storage of all bids.
* `collateralType` - id of the collateral type for which the `CollateralAuctionHouse` is responsible.
* `minimumBid` - minimum amount of system coins that must be submitted by each bidder.
* `totalAuctionLength` - auction length \(default: 7 days\).
* `auctionsStarted` - total auction count, used to track auction `id`s.
* `discount` - discount compared to the collateral market price; used when calculating the amount of collateral to send to a bidder.
* `lowerMedianDeviation` - max `median` collateral price deviation \(compared to the `osm` price\) used when the median price is lower than the `osm` price and the contract needs to pick which price to use.
* `upperMedianDeviation` - max `median` collateral price deviation \(compared to the `osm` price\) used when the median price is higher than the `osm` price and the contract needs to pick which price to use.
* `oracleRelayer` - address of the `OracleRelayer`.
* `osm` - collateral type `OSM` address.
* `median` - collateral type medianizer address.
* `RAD` - number with 45 decimals \(e.g a `cdpEngine.coinBalance`\)
* `WAD` - number with 18 decimals \(e.g a bid submitted when someone wants to buy collateral from an auction\)
* `RAY` - number with 27 decimals

**Data Structures**

* `Bid` - state of a specific auction
  * `raisedAmount` - amount of system coins raised up until this point
  * `soldAmount` - amount of collateral sold up until this point
  * `amountToSell` - quantity up for auction / collateral for sale
  * `amountToRaise` - total system coins wanted from the auction
  * `auctionDeadline` - max auction duration
  * `forgoneCollateralReceiver` - address of the CDP being auctioned. Receives collateral during the `decreaseSoldAmount` phase
  * `auctionIncomeRecipient` - recipient of auction income / receives system coin income \(this is the `AccountingEngine` contract\)

**Modifiers**

* `isAuthorized` ****- checks whether an address is part of `authorizedAddresses` \(and thus can call authed functions\).

**Functions**

* `modifyParameters(bytes32 parameter`, `uint256 data)` - update a `uint256` parameter.
* `modifyParameters(bytes32 parameter`, `address data)` - update an `address` parameter.
* `addAuthorization(usr: address)` - add an address to `authorizedAddresses`.
* `removeAuthorization(usr: address)` - remove an address from `authorizedAddresses`.
* `getDiscountedRedemptionCollateralPrice(osmPriceFeedValue: bytes32`,`medianPriceFeedValue: bytes32`, `customDiscount: uint256) public returns (uint256)` - get the collateral price according to the latest system coin `redemptionPrice` and after applying the `discount` to it.
* `getDiscountedRedemptionBoughtCollateral(id: uint256`, `osmPriceFeedValue: bytes32`, `medianPriceFeedValue: bytes32`, `adjustedBid: uint256) returns (uint256)` - get the amount of collateral that can be bought with a specific amount of system coins after calculating the discounted price \(and taking into consideration the system coin redemption, **not** market price\)
* `startAuction(forgoneCollateralReceiver: address`, `auctionIncomeRecipient: address`, `amountToRaise: uint256`, `amountToSell: uint256`, `initialBid: uint256 )` - function used by `LiquidationEngine` to start an auction / put collateral up for auction
* `getCollateralBought(id: uint256`, `wad: uint256) returns (uint256)` - get the amount of collateral that can be bought from a specific auction by bidding wad amount of system coins \(where `wad` will be scaled by `RAY` to transfer the correct amount of `RAD` system coins from `cdpEngine.coinBalance`\)
* `buyCollateral(id: uint256`, `wad: uint256)` - buy collateral from an auction and offer `wad` amount of system coins in return \(where `wad` is scaled by `RAY` in order to transfer `RAD` amount of coins from the bidder's `cdpEngine.coinBalance`\)
* `settleAuction(id: uint256)` - settle an auction that has passed its `auctionDeadline` and return any remaining collateral to the `forgoneCollateralReceiver`
* `terminateAuctionPrematurely(id: uint256)` - usually used during `GlobalSettlement` to terminate an auction early and send unsold collateral to the `msg.sender`
* `bidAmount(id: uint256) public view returns (uint256)` - always returns zero
* `remainingAmountToSell(id: uint256) public view returns (uint256)` - return the remaining collateral amount to sell from a specific auction
* `forgoneCollateralReceiver(uint id) public view returns (address)` - return the`forgoneCollateralReceiver` for a specific auction
* `amountToRaise(uint id) public view returns (uint256)` - return the amount of system coins to raise for a specific auction

**Events**

* `StartAuction`: emitted when `startAuction(address`, `address`, `uint256`, `uint256`, `uint256)` is successfully executed. Contains:
  * `id` - auction id
  * `amountToSell` - amount of collateral sold in the auction
  * `initialBid` - starting bid for the auction \(usually zero\).
  * `amountToRaise` - amount of system coins that should be raised by the auction.
  * `forgoneCollateralReceiver` - receiver of leftover collateral \(usually the CDP whose collateral was confiscated by the `LiquidationEngine`\).
  * `auctionIncomeRecipient` - receiver of system coins \(usually the `AccountingEngine`\).

## 3. Walkthrough <a id="3-key-mechanisms-and-concepts"></a>

The fixed discount auction is a straightforward way \(compared to `English` auctions\) to put CDP collateral up for sale in exchange for system coins used to settle bad debt. Bidders are only required to allow the auction house to transfer their `cdpEngine.coinBalance` and can then call`buyCollateral(id: uint256`, `wad: uint256)` in order to exchange their system coins for collateral which is sold at a `discount` compared to its latest recorded market price. Bidders can also review the \(approximate\) amount of collateral they can get from a specific auction by calling `getCollateralBought(id: uint256`, `wad: uint256)`.

{% hint style="info" %}
**Bids as WAD Amounts**

As opposed to `English` auctions where bidders submit bids with `RAD` amounts of system coins \(using `increaseBidSize` and `decreaseSoldAmount`\), `buyCollateral` requires a `WAD` amount of coins that are then multiplied by `RAY` in order to correctly scale the amount to `RAD`.
{% endhint %}

There are several components that come together when the contract calculates the amount of collateral to send to a bidder:

* The amount of system coins \(`wad`\) used when calling `buyCollateral`
* The current system coin `redemptionPrice`. The contract is not using the system coin's market price to determine the SYS\_COIN/COL price, thus eliminating the need for an oracle. On the other hand, bidders will need to take into account the deviation between the system coin's redemption and market prices
* The difference between the collateral's `OSM` price \(the delayed price\) and the latest market price stored in the medianizer \(which the `OSM` is connected to\). The auction house uses the `OSM` price by default so that, in case of an oracle attack, governance can react and temporarily disable the `OSM` \(in case they have power over that system component\) or any token holder can trigger settlement through the `ESM`. 
  * During the `OSM`'s delay, the market price \(and thus the medianizer value\) might change significantly and thus bidders might have to wait until a new median price feed is pushed into the `OSM` until they can profit from auctions. In order to minimize the number of cases where bidders have to wait for an `OSM` update and at the same time protect collateral auctions from an oracle attack, we added `lowerMedianDeviation` and `upperMedianDeviation` which allow the auction house to pick the medianizer price \(when calculating the amount of collateral bought by a bidder\) in case it deviated within certain bounds compared to the `OSM` price.

