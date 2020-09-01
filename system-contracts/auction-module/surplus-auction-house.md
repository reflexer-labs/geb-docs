---
description: >-
  Surplus auctioneer that sells extra stability fees in exchange for protocol
  tokens
---

# Surplus Auction House

## 1. Summary <a id="1-introduction-summary"></a>

The surplus auction is used to sell off a fixed amount of the surplus in exchange for protocol tokens. The surplus comes from the **stability fees** charged to SAFEs \(and stored in the `AccountingEngine`\). Bidders submit increasing amounts of protocol tokens and the winner receives all auctioned surplus in exchange for their coins which are burned.

There are two surplus auction flavours: a pre-settlement version meant to sell surplus before the system settles and a post-settlement one meant to sell any remaining surplus after global settlement is triggered.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `contractEnabled` - settlement flag \(available only in the pre-settlement surplus auction house\)
* `AUCTION_HOUSE_TYPE` - flag set to `bytes32("SURPLUS")`
* `authorizedAccounts[usr: address]` - addresses allowed to call `modifyParameters()` and `disableContract()`.
* `bids[id: uint]` - storage of all `Bid`s by `id`
* `safeEngine` - storage of the SAFEEngine's address
* `protocolToken` - address of the protocol token
* `auctionsStarted` - total auction count
* `bidDuration` - bid lifetime / max bid duration \(default: 3 hours\)
* `bidIncrease` - minimum bid increase \(default: 5%\)
* `totalAuctionLength` - maximum auction duration \(default: 2 days\)

**Data Structures**

* `Bid` - state of a specific auction
  * `bidAmount` - quantity being offered for the `amountToSell`
  * `amountToSell`- amount of surplus sold
  * `highBidder`
  * `bidExpiry`
  * `auctionDeadline` - when the auction will finish

**Modifiers**

* `isAuthorized` ****- checks whether an address is part of `authorizedAddresses` \(and thus can call authed functions\).

**Functions**

* `modifyParameters(bytes32 parameter`, `uint256 data)` - update a `uint256` parameter.
* `startAuction(amountToSell: uint256`, `initialBid: uint256)` - start a new surplus auction.
* `restartAuction(id: uint256)` - restart an auction if there have been 0 bids and the `auctionDeadline` has passed.
* `increaseBidSize(id: uint256`, `amountToBuy: uint256`, `bid: uint256)` - submit a bid with an increasing amount of protocol tokens for a fixed amount of system coins.
* `disableContract()` - disable the contract.
* `settleAuction(id: uint256)` - claim a winning bid / settles a completed auction.
* `terminateAuctionPrematurely(id: uint256)` - is used in case Governance wishes to upgrade \(only\) the `PreSettlementSurplusAuctionHouse` or in case `GlobalSettlement` is triggered. It settles `increaseBidSize` phase auctions, sending back the protocol tokens submitted by the `highBidder`. 

**Events**

* `AddAuthorization` - emitted when a new address becomes authorized. Contains:
  * `account` - the new authorized account
* `RemoveAuthorization` - emitted when an address is de-authorized. Contains:
  * `account` - the address that was de-authorized
* `ModifyParameters` - emitted after a parameter is modified
* `RestartAuction` - emitted after an auction is restarted. Contains:
  * `id` - the ID of the auction being restarted
  * `auctionDeadline` - the new auction deadline
* `IncreaseBidSize` - emitted when someone bids a higher amount of protocol tokens for the same amount of system coins. Contains:
  * `id` - the ID of the auction that's being bid on
  * `highBidder` - the new high bidder
  * `amountToBuy` - the amount of system coins to buy
  * `bid` - the amount of protocol tokens bid
  * `bidExpiry` - the new deadline when the auction will end which can be before the original `auctionDeadline`
* `StartAuction`- emitted when `startAuction(uint256`, `uint256)` is successfully executed. Contains:
  * `id` - auction id
  * `auctionsStarted` - total amount of auctions that have started up until now
  * `amountToSell` - amount of system coins sold  in the auction.
  * `initialBid` - starting bid for the auction
  * `auctionDeadline` - the auction's deadline
* `SettleAuction` - emitted after an auction is settled. Contains:
  * `id` - the ID of the auction that was settled
* `DisableContract` - emitted after the contract is disabled
* `TerminateAuctionPrematurely` - emitted after an auction is terminated before its deadline. Contains:
  * `id` - the ID of the auction that was terminated
  * `sender` - the address that terminated the auction
  * `highBidder` - the auction's high bidder
  * `bidAmount` - the latest bid amount

## 3. Walkthrough <a id="3-key-mechanisms-and-concepts"></a>

In a surplus auction, bidders compete for a fixed `amountToSell` of system coins with increasing `bidAmount`s of protocol tokens.

The surplus auction ends when the last bid's duration is passed \(`bidDuration`\) without another bid getting placed or when the auction duration \(`totalAuctionLength`\) has been surpassed. When the auction settles, the protocol tokens received are burnt.

In case governance disables the `PreSettlementSurplusAuctionHouse` by calling `disableContract`, anyone can call `terminateAuctionPrematurely` in order to quickly settle an auction and return the protocol token bid to `highBidder`. 

Governance cannot disable the `PostSettlementSurplusAuctionHouse` because it is the main component in charge with disbursing remaining surplus after the protocol shuts down.

