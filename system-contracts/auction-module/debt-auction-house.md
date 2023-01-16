---
description: >-
  Debt auctioneer that covers deficit by minting protocol tokens in exchange for
  system coins
---

# Debt Auction House

## 1. Summary <a href="#1-introduction-summary" id="1-introduction-summary"></a>

Debt auctions are used to recapitalize the system by auctioning off protocol tokens for a fixed amount of system coins. In this process, bidders compete by offering to accept decreasing amounts of protocol tokens for the coins they will end up paying.

## 2. Contract Variables & Functions <a href="#2-contract-details" id="2-contract-details"></a>

**Variables**

* `contractEnabled` - settlement flag (`1` or `0`).
* `AUCTION_HOUSE_TYPE` - flag set to `bytes32("DEBT")`
* `authorizedAccounts[usr: address]` - addresses allowed to call `modifyParameters()` and `disableContract()`.
* `safeEngine` - storage of the `SAFEEngine`'s address.
* `protocolToken` - token minted in exchange for system coins.
* `accountingEngine` - address of the `AccountingEngine` (receiver of system coins).
* `bids[id: uint]` - storage of all bids.
* `bidDuration` - bid lifetime.
* `bidDecrease` - minimum bid decrease.
* `amountSoldIncrease` - increase for size of `amountToSell` during `restartAuction` (default to 50%).
* `totalAuctionLength` - maximum auction duration.
* `auctionsStarted` - number of auctions that have started up until now.

**Data Structures**

* `Bid` - state of a specific auction. Contains:
  * `bidAmount` - paid system coins
  * `amountToSell` - quantity of protocol tokens up for auction
  * `highBidder`
  * `bidExpiry` - when a bid expires (and the auction ends)
  * `auctionDeadline` - max auction duration

**Modifiers**

* `isAuthorized` **** - checks whether an address is part of `authorizedAddresses` (and thus can call authed functions).

**Functions**

* `modifyParameters(bytes32 parameter`, `uint256 data)` - update a `uint256` parameter.
* `modifyParameters(bytes32 parameter`, `address data)` - update an `address` parameter.
* `startAuction(incomeReceiver: address`, `amountToSell: uint256`, `initialBid: uint256)` - start a new debt auction.
* `restartAuction(id: uint256)` - restart an auction if there have been 0 bids and the `auctionDeadline` has passed.
* `decreaseSoldAmount(id: uint256`, `amountToBuy: uint256`, `bid: uint256)` - submit a fixed system coin bid with an increasingly lower amount of protocol tokens you are willing to accept in exchange.
* `disableContract()` - disable the contract.
* `settleAuction(id: uint256)` - claim a winning bid / settles a completed auction
*   `terminateAuctionPrematurely(id: uint256)` - used during `GlobalSettlement` to terminate&#x20;

    `decreaseSoldAmount`phase auctions and repay system coins (using `safeEngine.createUnbackedDebt` with the `accountingEngine` as the `debtDestination`) to the highest bidder.

**Events**

* `AddAuthorization` - emitted when a new address becomes authorized. Contains:
  * `account` - the new authorized account
* `RemoveAuthorization` - emitted when an address is de-authorized. Contains:
  * `account` - the address that was de-authorized
* `StartAuction`- emitted when `startAuction(address`, `uint256`, `uint256)` is successfully executed. Contains:
  * `id` - auction id
  * `auctionsStarted` - total amount of auctions that have started up until now
  * `amountToSell` - amount of protocol tokens sold  in (to be minted after) the auction.
  * `initialBid` - starting bid for the auction.
  * `incomeReceiver` **** - address that receives the system coins from an auction (usually the `AccountingEngine`)
  * `auctionDeadline` - deadline for the auction with ID `id`
  * `activeDebtAuctions` - the current number of active debt auctions
* `ModifyParameters` - emitted after a parameter is modified
* `RestartAuction` - emitted after an auction is restarted. Contains:
  * `id` - the ID of the auction being restarted
  * `auctionDeadline` - the new auction deadline
* `DecreaseSoldAmount` - emitted when someone bids a smaller amount of protocol tokens for the same amount of system coins given in return. Contains:
  * `id` - the ID of the auction that's being bid on
  * `highBidder` - the new high bidder
  * `amountToBuy` - the protocol token bid
  * `bid` - the amount of system coins offered
  * `bidExpiry` - the timestamp when the auction will end even if it's earlier than the `auctionDeadline`
* `SettleAuction` - emitted after an auction is settled. Contains:
  * `id` - the ID of the settled auction
  * `activeDebtAuctions` - the new number of active debt auctions
* `TerminateAuctionPrematurely` - emitted after an auction is settled before its official deadline. Contains:
  * `id` - the ID of the auction that was terminated
  * `sender` - the address that terminated the auction
  * `highBidder` - the auction's high bidder
  * `bidAmount` - the auction's bid amount
  * `activeDebtAuctions` - the new number of active debt auctions
* `DisableContract` - emitted after the contract is disabled. Contains:
  * `sender` - the address that disabled the contract

## 3. Walkthrough <a href="#3-key-mechanisms-and-concepts" id="3-key-mechanisms-and-concepts"></a>

The `DebtAuctionHouse` is a reverse auction, meaning that participants bid with increasingly lower amounts of protocol tokens they are willing to accept for a fixed amount of system coins. The auction will end when the latest bid duration (`bidDuration`) has passed since the last submitted bid or when the general auction deadline (`auctionStartTime + totalAuctionLength`) has been reached.&#x20;

The first bidder will pay back the system debt (that the auction tries to cover) and submit their preference for the amount of protocol tokens they would like to receive. Each subsequent bid will pay back the previous (no longer winning) bidder. When the auction is over, the process ends by cleaning up the bid and minting protocol tokens for the winning bidder.

If the auction expires without receiving any bids, anyone can restart the auction by calling `restartAuction(uint auction_id)`. This will do two things:

1. It resets `bids[id].auctionDeadline` to `now + totalAuctionLength`
2. It resets `bids[id].amountToSell` to `bids[id].amountToSell * amountSoldIncrease / ONE`&#x20;

## 4. Gotchas (Potential Source of User Error)

#### **Keepers**

In the context of running a keeper (more info [here](https://github.com/reflexer-laps/geb-docs/tree/master/keepers)) to perform bids within an auction, a primary failure mode would occur when a keeper specifies an unprofitable price for FLX.

* This failure mode is due to the fact that there is nothing the system can do stop a user from paying significantly more than the fair market value for the token in an auction (this goes for all auction types, `collateral`, `debt`, and `surplus`).
* This means, in the case of Flop, that since the Coin amount is fixed for the entire auction, the risk to the keeper is that they would make a "winning" bid that pays the bid amount in Coin but does not receive any FLX (`amountToSell` == 0). Subsequent executions of this bad strategy would be limited by the amount of Coin (not FLX) in their vat balance.

## 5. Failure Modes (Bounds on Operating Conditions & External Risk Factors)

`DebtAuctionHouse` has the potential to issue an excessively huge amount of FLX and despite the mitigation efforts (the addition of the `initialDebtAuctionMintedTokens` and `amountSoldIncrease` parameters), if `initialDebtAuctionMintedTokens` is not set correctly by governance, the huge issuance of FLX could still occur.
