---
description: >-
  Debt auctioneer that covers deficit by offering protocol tokens in exchange
  for system coins
---

# Debt Auction House

## 1. Summary <a id="1-introduction-summary"></a>

Debt Auctions are used to recapitalize the system by auctioning off protocol tokens for a fixed amount of system coins. In this process, bidders compete by offering to accept decreasing amounts of protocol tokens for the coins they will end up paying.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

* `authorizedAccounts [usr: address]`, `addAuthorization`/`removeAuthorization`/`isAuthorized` - auth mechanisms
* `amountToSell`: quantity up for auction / protocol tokens for sale
* `highBidder`
* `auctionIncomeRecipient`: recipient of auction income / receives system coin income
* `bidDuration`: bid lifetime
* `bidDecrease`: minimum bid decrease
* `amountSoldIncrease`: Increase for size of `amountToSell` during `restartAuction` \(default to 50%\)
* `totalAuctionLength`: maximum auction duration
* `auctionDeadline`: when the auction will finish / max auction duration
* `startAuction`: start an auction / Put up a new protocol tokens for auction
* `decreaseSoldAmount`: make a bid, decreasing the amount sold
* `settleAuction`: claim a winning bid
* `cdpEngine` - the `CDPEngine`'s address
* `protocolToken`
* `auctionsStarted` - total auction count, used to track auction `id`s
* `contractEnabled` - settlement flag
* `Bid` - state of a specific Auction
* `bidAmount` - latest amount of system coins being big
* `bidExpiry`
* `restartAuction` - restarts an auction

## 3. Walkthrough <a id="3-key-mechanisms-and-concepts"></a>

The `DebtAuctionHouse` is a reverse auction, meaning that participants bid with increasingly lower amounts of protocol tokens they are willing to accept for a fixed amount of system coins. The auction will end when the latest bid duration \(`bidDuration`\) has passed since the last submitted bid or when the general auction deadline \(`auctionStartTime + totalAuctionLength`\) has been reached. 

The first bidder will pay back the system debt \(that the auction tries to cover\) and submit their preference for the amount of protocol tokens they would like to receive. Each subsequent bid will pay back the previous \(no longer winning\) bidder. When the auction is over, the process ends by cleaning up the bid and minting protocol tokens for the winning bidder.

If the auction expires without receiving any bids, anyone can restart the auction by calling `restartAuction(uint auction_id)`. This will do two things:

1. It resets `bids[id].auctionDeadline` to `now + totalAuctionLength`
2. It resets `bids[id].amountToSell` to `bids[id].amountToSell * amountSoldIncrease / ONE` 

