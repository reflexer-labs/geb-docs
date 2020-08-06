---
description: English collateral auctioneer that tries to recapitalize the system
---

# English Collateral Auction House

## 1. Summary <a id="1-introduction-summary"></a>

English collateral auctions are used to sell collateral from CDPs that have become undercollateralized in order to preserve the overall system health. The `LiquidationEngine` sends collateral to the `EnglishCollateralAuctionHouse` where it is auctioned in two phases: `increaseBidSize` and `decreaseSoldAmount`.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `contractEnabled` - settlement flag \(`1` or `0`\).
* `AUCTION_HOUSE_TYPE` - flag set to `bytes32("COLLATERAL")`
* `AUCTION_TYPE` - flag set to `bytes32("ENGLISH")`.
* `authorizedAccounts[usr: address]` - addresses allowed to call `modifyParameters()` and `disableContract()`.
* `cdpEngine` - storage of the `CDPEngine`'s address.
* `bids[id: uint]` - storage of all bids.
* `collateralType` - id of the collateral type for which the `CollateralAuctionHouse` is responsible.
* `bidIncrease` - minimum bid increase \(default: 5%\).
* `bidDuration` - bid duration \(default: 3 hours\).
* `totalAuctionLength` - auction length \(default: 2 days\).
* `auctionsStarted` - total auction count, used to track auction `id`s.
* `bidToMarketPriceRatio` - the minimum size of the first bid compared to the latest recorded collateral price ****\(for `collateralType`\) in the system.
* `oracleRelayer` - address of the `OracleRelayer`.
* `osm` - collateral type `OSM` address.

**Data Structures**

* `Bid` - state of a specific auction
  * `bidAmount` - paid system coins
  * `amountToSell` - quantity up for auction / collateral for sale
  * `highBidder`
  * `bidExpiry` - when a bid expires \(and the auction ends\)
  * `auctionDeadline` - max auction duration
  * `forgoneCollateralReceiver` - address of the CDP being auctioned. Receives collateral during the `decreaseSoldAmount` phase
  * `auctionIncomeRecipient` - recipient of auction income / receives system coin income \(this is the `AccountingEngine` contract\)
  * `amountToRaise` - total system coins wanted from the auction

**Modifiers**

* `isAuthorized` ****- checks whether an address is part of `authorizedAddresses` \(and thus can call authed functions\).

**Functions**

* `modifyParameters(bytes32 parameter`, `uint256 data)` - update a `uint256` parameter.
* `modifyParameters(bytes32 parameter`, `address data)` - update an `address` parameter.
* `addAuthorization(usr: address)` - add an address to `authorizedAddresses`.
* `removeAuthorization(usr: address)` - remove an address from `authorizedAddresses`.
* `startAuction(forgoneCollateralReceiver: address`, `auctionIncomeRecipient: address`, `amountToRaise: uint256`, `amountToSell: uint256`, `initialBid: uint256 )` - function used by `LiquidationEngine` to start an auction / put collateral up for auction
* `restartAuction(id: uint256)` - restart an auction if there have been 0 bids and the `auctionDeadline` has passed
* `increaseBidSize(id: uint256`, `amountToBuy: uint256`, `bid: uint256)` - first phase of an auction. Increasing system coin `bid`s for a set `amountToSell` of collateral
* `decreaseSoldAmount(id: uint256`, `amountToBuy: uint256`, `bid: uint256)` - second phase of an auction. Set system coin `bid` for a decreasing`amountToSell`of collateral.
* `settleAuction(id: uint256)` - claim a winning bid / settles a completed auction
* `terminateAuctionPrematurely(id: uint256)` - used during `GlobalSettlement` to terminate `increaseBidSize` phase auctions and transfer the collateral to the settlement contract while repaying system coins \(the winning bid\) to the highest bidder.

**Events**

* `StartAuction`: emitted when `startAuction(address`, `address`, `uint256`, `uint256`, `uint256)` is successfully executed. Contains:
  * `id` - auction id
  * `amountToSell` - amount of collateral sold in the auction
  * `initialBid` - starting bid for the auction \(usually zero\).
  * `amountToRaise` - amount of system coins that should be raised by the auction.
  * `forgoneCollateralReceiver` - receiver of leftover collateral \(usually the CDP whose collateral was confiscated by the `LiquidationEngine`\).
  * `auctionIncomeRecipient` - receiver of system coins \(usually the `AccountingEngine`\).

## 3. Walkthrough <a id="3-key-mechanisms-and-concepts"></a>

Starting in the `increaseBidSize`-phase, bidders compete for an `amountToSell` of collateral with increasing bid amounts of system coins. The very first `bidAmount` will need to be higher than or equal to `latest collateral price in collateral FSM * bidToMarketPriceRatio / RAY.` This ensures that no bidder can submit an extremely small `bidAmount` and thus pay a small price for the entire `amountToSell` collateral. 

Once `amountToRaise` amount of system coins has been raised, the auction moves to the `decreaseSoldAmount`-phase. This phase is meant to incentivize bidders to returns as much collateral as possible back to the CDP while still paying `amountToRaise` system coins.

Once the auction's last bid has expired or the auction itself has reached the `auctionDeadline` anyone can call `settleAuction` to payout the highest bidder \(`Bid.highBidder`\). This moves collateral from the `CollateralAuctionHouse`'s balance in the `CDPEngine` to the winning bidder's balance.

