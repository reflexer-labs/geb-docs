---
description: >-
  Auctioneer that tries to recapitalize the system by selling collateral at a
  discounted price
---

# Fixed Discount Collateral Auction House

## 1. Summary <a href="#1-introduction-summary" id="1-introduction-summary"></a>

Fixed discount collateral auctions are similar to their `English` counterpart in that they are used to preserve the overall system health by liquidating under-collateralized SAFEs and selling off collateral in exchange for system coins. This auction type automatically calculates an amount of collateral to send back to a bidder, taking into account the amount of system coins the bidder submits as well as the current system coin `redemptionPrice` (and optionally its market price) and collateral market price.

## 2. Contract Variables & Functions <a href="#2-contract-details" id="2-contract-details"></a>

**Variables**

* `contractEnabled` - settlement flag (`1` or `0`).
* `AUCTION_HOUSE_TYPE` - flag set to `bytes32("COLLATERAL")`
* `AUCTION_TYPE` - flag set to `bytes32("FIXED_DISCOUNT")`.
* `authorizedAccounts[usr: address]` - addresses allowed to call `modifyParameters()` and `disableContract()`.
* `safeEngine` - storage of the `SAFEEngine`'s address.
* `bids[id: uint]` - storage of all bids.
* `collateralType` - id of the collateral type for which the `CollateralAuctionHouse` is responsible.
* `minimumBid` - minimum amount of system coins that must be submitted by each bidder.
* `totalAuctionLength` - auction length (default: `uint48(-1)`).
* `auctionsStarted` - total auction count, used to track auction `id`s.
* `lastReadRedemptionPrice` - the last read redemption price. Can (and most probably is) be different than the latest `OracleRelayer._redemptionPrice`
* `discount` - discount compared to the collateral market price; used when calculating the amount of collateral to send to a bidder.
* `lowerCollateralMedianDeviation` - max `collateralMedian` collateral price deviation (compared to the `FSM` price) used when the median price is lower than the `collateralFSM` price and the contract needs to pick which one to use
* `upperCollateralMedianDeviation` - max `collateralMedian` collateral price deviation (compared to the `FSM` price) used when the median price is higher than the `collateralFSM` price and the contract needs to pick which one to use
* `lowerSystemCoinMedianDeviation` - max `systemCoinOracle` price deviation (compared to the `redemptionPrice`) used when the system coin's `redemptionPrice` price is higher than its market price and the contract needs to pick which one to use
* `upperSystemCoinMedianDeviation` - max `systemCoinOracle` price deviation (compared to the `redemptionPrice`) used when the system coin's `redemptionPrice` price is lower than its market price and the contract needs to pick which one to use
* `minSystemCoinMedianDeviation` - minimum deviation between the system coin's market and redemption prices that must be passed in order for the contract to use the deviated price instead of the redemption one
* `oracleRelayer` - address of the `OracleRelayer`
* `collateralFSM` - the collateral type's `FSM` address
* `collateralMedian` - collateral type medianizer address
* `systemCoinOracle` - market price oracle for the system coin
* `liquidationEngine` - the address of the `LiquidationEngine`
* `RAD` - number with 45 decimals (e.g a `safeEngine.coinBalance`)
* `WAD` - number with 18 decimals (e.g a bid submitted when someone wants to buy collateral from an auction)
* `RAY` - number with 27 decimals

**Data Structures**

* `Bid` - state of a specific auction
  * `raisedAmount` - amount of system coins raised up until this point
  * `soldAmount` - amount of collateral sold up until this point
  * `amountToSell` - quantity up for auction / collateral for sale
  * `amountToRaise` - total system coins wanted from the auction
  * `auctionDeadline` - max auction duration
  * `forgoneCollateralReceiver` - address of the SAFE being auctioned
  * `auctionIncomeRecipient` - recipient of auction income / receives system coin income (this is  the `AccountingEngine` contract)

**Modifiers**

* `isAuthorized` - checks whether an address is part of `authorizedAddresses` (and thus can call authed functions).

**Functions**

* `modifyParameters(bytes32 parameter`, `uint256 data)` - update an `uint256` parameter.
* `modifyParameters(bytes32 parameter`, `address data)` - update an `address` parameter.
* `addAuthorization(usr: address)` - add an address to `authorizedAddresses`.
* `removeAuthorization(usr: address)` - remove an address from `authorizedAddresses`.
* `getCollateralMedianPrice() public view returns (priceFeed: uint256)` - get the collateral's median price from `collateralMedian`
* `getSystemCoinMarketPrice() public view returns (priceFeed: uint256)` - get the system coin's  market price from `systemCoinOracle`
* `getFinalTokenPrices(systemCoinRedemptionPrice: uint256) public view returns (uint256`, `uint256)` - get the collateral and system coin prices that can be currently used to determine the amount of collateral bought by bidders
* `getFinalBaseCollateralPrice(collateralFsmPriceFeedValue: uint256`, `collateralMedianPriceFeedValue: uint256) public view returns (uint256)` - get the final collateral price (without the discount applied) that will be used to determine the amount of collateral bought by a bid
* `getDiscountedCollateralPrice(collateralFsmPriceFeedValue: bytes32`, `collateralMedianPriceFeedValue: bytes32`, `systemCoinPriceFeedValue: uint256`, `customDiscount: uint256) public view returns (uint256)` - get the (discounted) collateral price using either the `FSM` or median price for the collateral and the redemption/market price for the system coin
* `startAuction(forgoneCollateralReceiver: address`, `auctionIncomeRecipient: address`, `amountToRaise: uint256`, `amountToSell: uint256`, `initialBid: uint256 )` - function used by `LiquidationEngine` to start an auction / put collateral up for auction
* `getApproximateCollateralBought(id: uint256`, `wad: uint256)` - get the amount of collateral that can be bought from a specific auction by bidding `wad` and assuming that the latest system coin `redemptionPrice` is equal to `lastReadRedemptionPrice`
* `getCollateralBought(id: uint256`, `wad: uint256) returns (uint256`, `uint256)` - get the amount of collateral that can be bought from a specific auction by bidding `wad` amount of system coins (where `wad` will be scaled by `RAY` to transfer the correct amount of `RAD` system coins from `safeEngine.coinBalance`)
* `buyCollateral(id: uint256`, `wad: uint256)` - buy collateral from an auction and offer `wad` amount of system coins in return (where `wad` is scaled by `RAY` in order to transfer `RAD` amount of coins from the bidder's `safeEngine.coinBalance`)
* `settleAuction(id: uint256)` - settle an auction that has passed its `auctionDeadline` and return any unsold collateral to the `forgoneCollateralReceiver`
* `terminateAuctionPrematurely(id: uint256)` - normally used during `GlobalSettlement` to terminate an auction early and send unsold collateral to the `msg.sender` as well as call `LiquidationEngine` in order to subtract `bids[auctionId].amountToRaise` from `LiquidationEngine.currentOnAuctionSystemCoins`
* `bidAmount(id: uint256) public view returns (uint256)` - always returns zero
* `remainingAmountToSell(id: uint256) public view returns (uint256)` - return the remaining collateral amount to sell from a specific auction
* `forgoneCollateralReceiver(uint id) public view returns (address)` - returns the`forgoneCollateralReceiver` for a specific auction
* `raisedAmount(id: uint256)` - returns the currently raised amount of system coins for a specific auction.
* `amountToRaise(uint id) public view returns (uint256)` - returns the amount of system coins to raise for a specific auction

**Events**

* `AddAuthorization` - emitted when a new address becomes authorized. Contains:
  * `account` - the new authorized account
* `RemoveAuthorization` - emitted when an address is de-authorized. Contains:
  * `account` - the address that was de-authorized
* `StartAuction`- emitted when `startAuction(address`, `address`, `uint256`, `uint256`, `uint256)` is successfully executed. Contains:
  * `id` - auction id
  * `auctionsStarted` - amount of auctions started up until now
  * `amountToSell` - amount of collateral sold in the auction
  * `initialBid` - starting bid for the auction (usually zero).
  * `amountToRaise` - amount of system coins that should be raised by the auction.
  * `forgoneCollateralReceiver` - receiver of leftover collateral (usually the SAFE whose collateral was confiscated by the `LiquidationEngine`).
  * `auctionIncomeRecipient` - receiver of system coins (usually the `AccountingEngine`).
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

## 3. Walkthrough <a href="#3-key-mechanisms-and-concepts" id="3-key-mechanisms-and-concepts"></a>

The fixed discount auction is a straightforward way (compared to `English` auctions) to put SAFE collateral up for sale in exchange for system coins used to settle bad debt. Bidders are only required to allow the auction house to transfer their `safeEngine.coinBalance` and can then call`buyCollateral(id: uint256`, `wad: uint256)` in order to exchange their system coins for collateral which is sold at a `discount` compared to its latest recorded market price. Bidders can also review the amount of collateral they can get from a specific auction by calling `getCollateralBought(id: uint256`, `wad: uint256)` or `getApproximateCollateralBought(id: uint256`, `wad: uint256)`. Note that `getCollateralBought` is not marked as `view` because it reads (and also updates) the `redemptionPrice` from the `OracleRelayer` whereas `getApproximateCollateralBought` uses the`lastReadRedemptionPrice`.

{% hint style="info" %}
**Bids as WAD Amounts**

As opposed to `English` auctions where bidders submit bids with `RAD` amounts of system coins (using `increaseBidSize` and `decreaseSoldAmount`), `buyCollateral` requires a `WAD` amount of coins that are then multiplied by `RAY` in order to correctly scale the amount to `RAD` and transfer coins from the bidder's `safeEngine.coinBalance.`
{% endhint %}

There are several parameters that come together when the contract calculates the amount of collateral to send to a bidder:

* The amount of system coins `wad` used when calling `buyCollateral`. `wad` must be bigger than zero and bigger than or equal to `minimumBid`. In case `wad` is bigger than `minimumBid`, the contract will only request what it needs in order to fill `remainingToRaise`. Note that in order to avoid dusty auctions resulting from computing an `adjustedBid` (because of precision loss from division) the contract will automatically request `10**-18` extra system coins
* The difference between the collateral's `FSM` price (the delayed price) and the latest market price stored in the medianizer (which the `FSM` is connected to). The auction house uses the `FSM` price by default so that, in case of an oracle attack, governance can react and temporarily disable the `FSM` (in case they have power over that system component).
  * During the `FSM`'s delay, the market price (and thus the medianizer value) might change significantly and thus bidders might have to wait until a new median price feed is pushed into the `FSM` so that they can profit from auctions. In order to avoid scenarios where bidders have to wait for an `FSM` update and at the same time protect collateral auctions from an oracle attack, we added two variables: `lowerCollateralMedianDeviation`and `upperCollateralMedianDeviation`which allow the auction house to pick the medianizer price (when calculating the amount of collateral bought by a bidder) in case it deviated within certain bounds compared to the `FSM` price.
* The difference between the system coin's `redemptionPrice` and its market price (provided only if `systemCoinOracle` is not null and it returns a valid price). The auction house uses the `redemptionPrice` by default, although, during market shocks, there may be a significant difference between the system coin's redemption and market prices which will discourage bidding (if the market price is above redemption). This is why governance can set `lowerSystemCoinMedianDeviation` and `upperSystemCoinMedianDeviation` in order to allow the contract to use the system coin market price if it deviated within certain bounds from the `redemptionPrice`. Governance can also set `minSystemCoinMedianDeviation` so that the contract only chooses the market price in case it deviated above a certain threshold.

Similar to `English` auctions, when the auction is settled (or terminated prematurely), the contract will call the `LiquidationEngine` in order to `removeCoinsFromAuction` (subtract `bids[auctionId].amountToRaise` from `LiquidationEngine.currentOnAuctionSystemCoins`).

## 4. Gotchas <a href="#3-key-mechanisms-and-concepts" id="3-key-mechanisms-and-concepts"></a>

* In case someone bids an amount that is higher than the remaining amount of system coins which still need to be raised by an auction, the contract will only charge for the remaining amount plus `10**-18` extra system coins (meant to prevent dusty auctions)
* You must always submit a bid that is higher than or equal to `minimum(minimumBid`, `subtract(bids[id].amountToRaise`, `bids[id].raisedAmount))`. The contract will take care of charging only for the amount needed to cover the total remaining`amountToRaise`
* The auctions use the system coin `redemptionPrice` by default (not its market price)

## 5. Examples <a href="#3-key-mechanisms-and-concepts" id="3-key-mechanisms-and-concepts"></a>

```
// Scenario 1

minimumBid                     = 5 * WAD  // 5 coins
discount                       = 0.95E18  // 5%
lowerCollateralMedianDeviation = 0.90E18  // 10%
upperCollateralMedianDeviation = 0.95E18  // 5%
lowerSystemCoinMedianDeviation = WAD      // 0%
upperSystemCoinMedianDeviation = WAD      // 0%
minSystemCoinMedianDeviation   = 0.999E18 // 0.1%

collateralFsmPriceFeedValue    = 100 * WAD
collateralMedianPriceFeedValue = 89 * WAD

systemCoinRedemptionPrice      = 5 * RAY
systemCoinMarketPrice          = 5.01 * RAY

amountToSell                   = WAD
amountToRaise                  = 10 * RAD

submittedBid                   = 5 * WAD

/* 
    Because the collateral price fetched from the median is smaller than
    the collateral's FSM price and it is also deviated more than what
    lowerCollateralMedianDeviation permits (11%), the final collateral price 
    used by the contract will be 
    0.9 (lower collateral deviation) * 100 (collateral FSM price) = 90 USD
*/
finalCollateralPrice            = 90 * WAD

/*
    Even if the system coin market price is deviated from the redemption price,
    both the lower and the upper systemCoinMedianDeviation are 0% so the contract
    will use the redemption price
*/
finalSystemCoinPrice            = 5 * RAY

/*
    Determining the amount of collateral bought given the 5 system coin bid
*/
discountedSystemCoinCollateralPrice 
    = (finalCollateralPrice * RAY / finalSystemCoinPrice) * discount / WAD 
    = 17.1 * WAD

boughtCollateralAmount 
    = submittedBid * WAD / discountedSystemCoinCollateralPrice
    = 0.292397661 * WAD
```

```
// Scenario 2

minimumBid                     = 5 * WAD  // 5 coins
discount                       = 0.95E18  // 5%
lowerCollateralMedianDeviation = 0.90E18  // 10%
upperCollateralMedianDeviation = 0.95E18  // 5%
lowerSystemCoinMedianDeviation = 0.95E18  // 5%
upperSystemCoinMedianDeviation = 0.98E18  // 2%
minSystemCoinMedianDeviation   = 0.999E18 // 0.1%

collateralFsmPriceFeedValue    = 100 * WAD
collateralMedianPriceFeedValue = 89 * WAD

systemCoinRedemptionPrice      = 5 * RAY
systemCoinMarketPrice          = 5.1 * RAY

amountToSell                   = WAD
amountToRaise                  = 10 * RAD

submittedBid                   = 15 * WAD

/*
    Given that the submittedBid is higher than the total remaining amount to raise,
    the contract will adjust the bid (by rounding up)
*/
adjustedBid = amountToRaise / RAY + 1 = 10 * WAD + 1

/*
    Similar to Scenario 1, the final collateral price used by the contract 
    will be 90 USD
*/
finalCollateralPrice           = 90 * WAD

/*
    The system coin market price is 2% deviated compared to the redemption price
    and upperSystemCoinMedianDeviation is also 2% so the contract will pick
    5 (redemption price) * 1.02 (allowed deviation) = 5.1 USD as the system
    coin price
*/
finalSystemCoinPrice           = 5.1 * RAY

/*
    Determining the amount of collateral bought given the 10 WAD + 1 
    adjusted system coin bid
*/
discountedSystemCoinCollateralPrice 
    = (finalCollateralPrice * RAY / finalSystemCoinPrice) * discount / WAD 
    = 16.764705882 * WAD

boughtCollateralAmount 
    = adjustedBid * WAD / discountedSystemCoinCollateralPrice
    = 0.596491228082733148 * WAD
```
