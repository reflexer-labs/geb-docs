---
description: >-
  Debt auctioneer that covers deficit by minting protocol tokens in exchange for
  system coins
---

# Debt Auction House

## 1. Summary <a id="1-introduction-summary"></a>

Debt Auctions are used to recapitalize the system by auctioning off protocol tokens for a fixed amount of system coins. In this process, bidders compete by offering to accept decreasing amounts of protocol tokens for the coins they will end up paying.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `contractEnabled` - settlement flag \(`1` or `0`\).
* `AUCTION_HOUSE_TYPE` - flag set to `bytes32("DEBT")`
* `authorizedAccounts[usr: address]` - addresses allowed to call `modifyParameters()` and `disableContract()`.
* `cdpEngine` - storage of the `CDPEngine`'s address.
* `protocolToken` - token minted in exchange for system coins.
* `accountingEngine` - address of the `AccountingEngine` \(receiver of system coins\).
* `bids[id: uint]` - storage of all bids.
* `bidDuration` - bid lifetime.
* `bidDecrease` - minimum bid decrease.
* `amountSoldIncrease` - increase for size of `amountToSell` during `restartAuction` \(default to 50%\).
* `totalAuctionLength` - maximum auction duration.
* `auctionsStarted` - number of auctions that have started up until now.

**Data Structures**

* `Bid` - state of a specific auction. Contains:
  * `bidAmount` - paid system coins
  * `amountToSell` - quantity of protocol tokens up for auction
  * `highBidder`
  * `bidExpiry` - when a bid expires \(and the auction ends\)
  * `auctionDeadline` - max auction duration

**Modifiers**

* `isAuthorized` ****- checks whether an address is part of `authorizedAddresses` \(and thus can call authed functions\).

**Functions**

* `modifyParameters(bytes32 parameter`, `uint256 data)` - update a `uint256` parameter.
* `modifyParameters(bytes32 parameter`, `address data)` - update an `address` parameter.
* `startAuction(incomeReceiver: address`, `amountToSell: uint256`, `initialBid: uint256)` - start a new debt auction.
* `restartAuction(id: uint256)` - restart an auction if there have been 0 bids and the `auctionDeadline` has passed.
* `decreaseSoldAmount(id: uint256`, `amountToBuy: uint256`, `bid: uint256)` - submit a fixed system coin bid with an increasingly lower amount of protocol tokens you are willing to accept in exchange.
* `disableContract()` - disable the contract.
* `settleAuction(id: uint256)` - claim a winning bid / settles a completed auction
* `terminateAuctionPrematurely(id: uint256)` - used during `GlobalSettlement` to terminate 

  `decreaseSoldAmount`phase auctions and repay system coins \(using `cdpEngine.createUnbackedDebt` with the `accountingEngine` as the `debtDestination`\) to the highest bidder.

**Events**

* `StartAuction`: emitted when `startAuction(address`, `uint256`, `uint256)` is successfully executed. Contains:
  * `id` - auction id.
  * `amountToSell` - amount of protocol tokens sold  in \(to be minted after\) the auction.
  * `initialBid` - starting bid for the auction.
  * `incomeReceiver` ****- address that receives the system coins from an auction \(usually the `AccountingEngine`\).

## 3. Walkthrough <a id="3-key-mechanisms-and-concepts"></a>

The `DebtAuctionHouse` is a reverse auction, meaning that participants bid with increasingly lower amounts of protocol tokens they are willing to accept for a fixed amount of system coins. The auction will end when the latest bid duration \(`bidDuration`\) has passed since the last submitted bid or when the general auction deadline \(`auctionStartTime + totalAuctionLength`\) has been reached. 

The first bidder will pay back the system debt \(that the auction tries to cover\) and submit their preference for the amount of protocol tokens they would like to receive. Each subsequent bid will pay back the previous \(no longer winning\) bidder. When the auction is over, the process ends by cleaning up the bid and minting protocol tokens for the winning bidder.

If the auction expires without receiving any bids, anyone can restart the auction by calling `restartAuction(uint auction_id)`. This will do two things:

1. It resets `bids[id].auctionDeadline` to `now + totalAuctionLength`
2. It resets `bids[id].amountToSell` to `bids[id].amountToSell * amountSoldIncrease / ONE` 

