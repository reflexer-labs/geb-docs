---
description: >-
  Auctioneer that tries to recapitalize the system by selling collateral at an
  increasing discount
---

# Increasing Discount Collateral Auction House

## 1. Summary

Increasing discount collateral auctions are similar to fixed discount ones in that they are used to preserve the overall system health by liquidating under-collateralized SAFEs and selling off collateral at a discount. This auction type automatically calculates an amount of collateral to send back to a bidder, taking into account the amount of system coins the bidder submits as well as the current system coin `redemptionPrice` \(and optionally its market price\) and collateral market price.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `contractEnabled` - settlement flag \(`1` or `0`\).
* `AUCTION_HOUSE_TYPE` - flag set to `bytes32("COLLATERAL")`
* `AUCTION_TYPE` - flag set to `bytes32("INCREASING_DISCOUNT")`.
* `authorizedAccounts[usr: address]` - addresses allowed to call `modifyParameters()` and `disableContract()`.
* `safeEngine` - storage of the `SAFEEngine`'s address.
* `bids[id: uint]` - storage of all bids.
* `collateralType` - id of the collateral type for which the `CollateralAuctionHouse` is responsible.
* `minimumBid` - minimum amount of system coins that must be submitted by each bidder.
* `totalAuctionLength` - auction length \(default: `uint48(-1)`\).
* `auctionsStarted` - total auction count, used to track auction `id`s.
* `lastReadRedemptionPrice` - the last read redemption price. Can \(and most probably is\) be different than the latest `OracleRelayer._redemptionPrice`
* `minDiscount` - initial discount \(compared to the collateral market price\) used when calculating the amount of collateral to send to a bidder.
* `maxDiscount` - maximum discount \(compared to the collateral market price\) used when calculating the amount of collateral to send to a bidder.
* `perSecondDiscountUpdateRate` - the rate at which the discount will be updated in an auction.
* `maxDiscountUpdateRateTimeline` - max time over which the discount can be updated in an auction.
* `lowerCollateralMedianDeviation` - max `collateralMedian` collateral price deviation \(compared to the `FSM` price\) used when the median price is lower than the `collateralFSM` price and the contract needs to pick which one to use
* `upperCollateralMedianDeviation` - max `collateralMedian` collateral price deviation \(compared to the `FSM` price\) used when the median price is higher than the `collateralFSM` price and the contract needs to pick which one to use
* `lowerSystemCoinMedianDeviation` - max `systemCoinOracle` price deviation \(compared to the `redemptionPrice`\) used when the system coin's `redemptionPrice` price is higher than its market price and the contract needs to pick which one to use
* `upperSystemCoinMedianDeviation` - max `systemCoinOracle` price deviation \(compared to the `redemptionPrice`\) used when the system coin's `redemptionPrice` price is lower than its market price and the contract needs to pick which one to use
* `minSystemCoinMedianDeviation` - minimum deviation between the system coin's market and redemption prices that must be passed in order for the contract to use the deviated price instead of the redemption one
* `oracleRelayer` - the address of the `OracleRelayer`
* `collateralFSM` - the collateral type's `FSM` address
* `collateralMedian` - collateral type medianizer address
* `systemCoinOracle` - market price oracle for the system coin
* `liquidationEngine` - the address of the `LiquidationEngine`
* `RAD` - number with 45 decimals \(e.g a `safeEngine.coinBalance`\)
* `WAD` - number with 18 decimals \(e.g a bid submitted when someone wants to buy collateral from an auction\)
* `RAY` - number with 27 decimals

**Data Structures**

* `Bid` - state of a specific auction
  * `amountToSell` - quantity up for auction / remaining collateral for sale
  * `amountToRaise` - total system coins still requested by the auction
  * `currentDiscount` - the current discount being used in the auction
  * `maxDiscount` - the max value that the `currentDiscount` can have
  * `perSecondDiscountUpdateRate` - the rate at which the `currentDiscount` grows
  * `latestDiscountUpdateTime` - last time when the `currentDiscount` was updated
  * `discountIncreaseDeadline` - deadline after which the discount cannot increase anymore
  * `forgoneCollateralReceiver` - address of the SAFE whose collateral and debt were confiscated
  * `auctionIncomeRecipient` - recipient of auction income / receives system coin income \(this is  the `AccountingEngine` contract\)

**Modifiers**

* `isAuthorized` ****- checks whether an address is part of `authorizedAddresses` \(and thus can call authed functions\).

**Functions**

* `modifyParameters(bytes32 parameter`, `uint256 data)` - update an `uint256` parameter.
* `modifyParameters(bytes32 parameter`, `address data)` - update an `address` parameter.
* `addAuthorization(usr: address)` - add an address to `authorizedAddresses`.
* `removeAuthorization(usr: address)` - remove an address from `authorizedAddresses`.
* `getCollateralMedianPrice() public view returns (priceFeed: uint256)` - get the collateral's median price from `collateralMedian`
* `getSystemCoinFloorDeviatedPrice(redemptionPrice: uint256) public view returns` `(floorPrice: uint256)` - get the smallest possible price that's at max `lowerSystemCoinMedianDeviation` deviated from the redemption price and at least `minSystemCoinMedianDeviation` deviated
* `getSystemCoinCeilingDeviatedPrice(redemptionPrice: uint256) public view returns` `(ceilingPrice: uint256)` - get the highest possible price that's at max `upperSystemCoinMedianDeviation` deviated from the redemption price and at least `minSystemCoinMedianDeviation` deviated
* `getCollateralFSMAndFinalSystemCoinPrices(systemCoinRedemptionPrice: uint256) public` `view returns (uint256, uint256)` - get the collateral price from the FSM and the final system coin price that will be used when bidding in an auction
* `getSystemCoinMarketPrice() public view returns (priceFeed: uint256)` - get the system coin's  market price from `systemCoinOracle`
* `getFinalTokenPrices(systemCoinRedemptionPrice: uint256) public view returns (uint256`, `uint256)` - get the collateral and system coin prices that can be currently used to determine the amount of collateral bought by bidders
* `getFinalBaseCollateralPrice(collateralFsmPriceFeedValue: uint256`, `collateralMedianPriceFeedValue: uint256) public view returns (uint256)` - get the collateral price \(without the discount applied\) used in bidding by picking between the raw FSM and the oracle median price and taking into account deviation limits
* `getDiscountedCollateralPrice(collateralFsmPriceFeedValue: bytes32`, `collateralMedianPriceFeedValue: bytes32`, `systemCoinPriceFeedValue: uint256`, `customDiscount: uint256) public view returns (uint256)` - get the \(discounted\) collateral price using either the `FSM` or median price for the collateral and the redemption/market price for the system coin
* `getNextCurrentDiscount(id: uint256) public view returns (uint256)` -  get the upcoming discount that will be used in a specific auction
* `getAdjustedBid(id: uint256`, `wad: uint256) public view returns (bool, uint256)` - get the actual bid that will be used in an auction \(taking into account the bidder input\)
* `startAuction(forgoneCollateralReceiver: address`, `auctionIncomeRecipient: address`, `amountToRaise: uint256`, `amountToSell: uint256`, `initialBid: uint256 )` - function used by `LiquidationEngine` to start an auction / put collateral up for auction
* `getApproximateCollateralBought(id: uint256`, `wad: uint256)` - get the amount of collateral that can be bought from a specific auction by bidding `wad` sysem coins and assuming that the latest system coin `redemptionPrice` is equal to `lastReadRedemptionPrice`
* `getCollateralBought(id: uint256`, `wad: uint256) returns (uint256`, `uint256)` - get the amount of collateral that can be bought from a specific auction by bidding `wad` amount of system coins \(where `wad` will be scaled by `RAY` to transfer the correct amount of `RAD` system coins from `safeEngine.coinBalance`\)
* `buyCollateral(id: uint256`, `wad: uint256)` - buy collateral from an auction and offer `wad` amount of system coins in return \(where `wad` is scaled by `RAY` in order to transfer `RAD` amount of coins from the bidder's `safeEngine.coinBalance`\)
* `settleAuction(id: uint256)` - settle an auction that has passed its `auctionDeadline` and return any unsold collateral to the `forgoneCollateralReceiver`
* `terminateAuctionPrematurely(id: uint256)` - normally used during `GlobalSettlement` to terminate an auction early and send unsold collateral to the `msg.sender` as well as call `LiquidationEngine` in order to subtract `bids[auctionId].amountToRaise` from `LiquidationEngine.currentOnAuctionSystemCoins`
* `bidAmount(id: uint256) public view returns (uint256)` - always returns zero.
* `remainingAmountToSell(id: uint256) public view returns (uint256)` - return the remaining collateral amount to sell from a specific auction.
* `forgoneCollateralReceiver(uint id) public view returns (address)` - returns the`forgoneCollateralReceiver` for a specific auction.
* `raisedAmount(id: uint256)` - returns zero.
* `amountToRaise(uint id) public view returns (uint256)` - returns the amount of system coins left to raise by a specific auction.

**Events**

* `AddAuthorization` - emitted when a new address becomes authorized. Contains:
  * `account` - the new authorized account
* `RemoveAuthorization` - emitted when an address is de-authorized. Contains:
  * `account` - the address that was de-authorized
* `StartAuction`- emitted when `startAuction(address`, `address`, `uint256`, `uint256`, `uint256)` is successfully executed. Contains:
  * `id` - auction id
  * `auctionsStarted` - amount of auctions started up until now
  * `amountToSell` - amount of collateral sold in the auction
  * `initialBid` - starting bid for the auction \(usually zero\).
  * `amountToRaise` - amount of system coins that should be raised by the auction.
  * `forgoneCollateralReceiver` - receiver of leftover collateral \(usually the SAFE whose collateral was confiscated by the `LiquidationEngine`\).
  * `auctionIncomeRecipient` - receiver of system coins \(usually the `AccountingEngine`\).
  * `auctionDeadline` - the auction's deadline
* `ModifyParameters` - emitted when a parameter is modified
* `BuyCollateral` - emitted when someone buys collateral from an auction. Contains:
  * `id` - the ID of the auction from which collateral was bought
  * `wad` - the bid size
  * `boughtCollateral` - the amount of collateral that was bought
* `SettleAuction` - emitted when someone settles an auction. Contains:
  * `id` - the ID of the auction settled
  * `leftoverCollateral` - the amount of collateral that hasn't been sold by the now settled auction
* `TerminateAuctionPrematurely` - emitted when an auction is terminated before its deadline. Contains:
  * `id` - the ID of the auction that was terminated
  * `sender` - the address that terminated the auction
  * `collateralAmount` - the amount of collateral still unauctioned

