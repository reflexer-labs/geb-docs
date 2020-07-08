---
description: >-
  Surplus auctioneer that sells extra stability fees in exchange for protocol
  tokens
---

# Surplus Auction House

## 1. Summary <a id="1-introduction-summary"></a>

The surplus auction is used to sell off a fixed amount of the surplus in exchange for protocol tokens. The surplus comes from the **stability fees** charged to CDPs \(and stored in the `AccountingEngine`\). Bidders submit increasing amounts of protocol tokens and the winner receives all auctioned surplus in exchange for their coins which are burned.

There are two surplus auction flavours: a pre-settlement version meant to sell surplus before the system settles and a post-settlement one meant to sell any remaining surplus after global settlement is triggered.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

* `authorizedAccounts [usr: address]` - `addAuthorization`/`removeAuthorization`/`isAuthorized` auth mechanisms
* `Bid` - state of a specific auction
  * `bidAmount` - quantity being offered for the `amountToSell` \(protocol tokens\)
  * `amountToSell`- amount of surplus sold
  * `highBidder`
  * `bidExpiry`
  * `auctionDeadline` - when the auction will finish
* `bids (id: uint)` - storage of all `Bid`s by `id`
* `cdpEngine` - storage of the CDPEngine's address
* `bidDuration` - bid lifetime / max bid duration \(default: 3 hours\) \[uint48\]
* `bidIncrease` - minimum bid increase \(default: 5%\)
* `totalAuctionLength` - maximum auction duration \(default: 2 days\)
* `startAuction` - start an auction / put up a new system coin `amountToSell` for auction
* `increaseBidSize` - make a bid, thus increasing the bid size / submit a protocol token bid \(increasing `bidAmount`\)
* `settleAuction` - claim a winning bid / settling a completed auction
* `protocolToken`
* `auctionsStarted` - total auction count
* `contractEnabled` - settlement flag \(available only in the pre-settlement surplus auction house\)
* `modifyParameters` - used by governance to set auction parameters
* `terminateAuctionPrematurely` - is used in case Governance wishes to upgrade \(only\) the `PreSettlementSurplusAuctionHouse` or in case `GlobalSettlement` is triggered. It settles `increaseBidSize` phase auctions, sending back the protocol tokens submitted by the `highBidder`. 
* `restartAuction` **-** resets the `auctionDeadline` value if an auction received no bids and the original `auctionDeadline`has passed.

## 3. Walkthrough <a id="3-key-mechanisms-and-concepts"></a>

In a surplus auction, bidders compete for a fixed `amountToSell` of system coins with increasing `bidAmount`s of protocol tokens.

The surplus auction ends when the last bid's duration is passed \(`bidDuration`\) without another bid getting placed or when the auction duration \(`totalAuctionLength`\) has been surpassed. When the auction settles, the protocol tokens received are burnt.

In case governance disables the `PreSettlementSurplusAuctionHouse` by calling `disableContract`, anyone can call `terminateAuctionPrematurely` in order to quickly settle an auction and return the protocol token bid to `highBidder`. 

Governance cannot disable the `PostSettlementSurplusAuctionHouse` because it is the main component in charge with disbursing remaining surplus after the protocol shuts down.

