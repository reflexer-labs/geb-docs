---
description: >-
  Surplus auctioneer that sells extra stability fees in exchange for protocol
  tokens
---

# Surplus Auction House

## 1. Summary <a href="#1-introduction-summary" id="1-introduction-summary"></a>

The surplus auction is used to sell off a fixed amount of the surplus in exchange for protocol tokens. The surplus comes from the stability fees charged to SAFEs (and stored in the `AccountingEngine`). Bidders submit increasing amounts of protocol tokens and the winner receives all auctioned surplus in exchange for their coins which are burned or transferred to another address.

## 2. Contract Variables & Functions <a href="#2-contract-details" id="2-contract-details"></a>

**Variables**

* `contractEnabled` - settlement flag (available only in the pre-settlement surplus auction house)
* `AUCTION_HOUSE_TYPE` - flag set to `bytes32("SURPLUS")`
* `authorizedAccounts[usr: address]` - addresses allowed to call `modifyParameters()` and `disableContract()`.
* `bids[id: uint]` - storage of all `Bid`s by `id`
* `safeEngine` - storage of the `SAFEEngine`'s address
* `protocolToken` - address of the protocol token
* `auctionsStarted` - total auction count
* `bidDuration` - bid lifetime / max bid duration (default: 3 hours)
* `bidIncrease` - minimum bid increase (default: 5%)
* `totalAuctionLength` - maximum auction duration (default: 2 days)
* `protocolTokenBidReceiver` - receiver of protocol tokens after an auction is settled. Only present in the `RecyclingSurplusAuctionHouse`.

**Data Structures**

* `Bid` - state of a specific auction
  * `bidAmount` - quantity being offered for the `amountToSell`
  * `amountToSell`- amount of surplus sold
  * `highBidder`
  * `bidExpiry`
  * `auctionDeadline` - when the auction will finish

**Modifiers**

* `isAuthorized` **** - checks whether an address is part of `authorizedAddresses` (and thus can call authed functions).

**Functions**

* `modifyParameters(bytes32 parameter`, `uint256 data)` - update a `uint256` parameter.
* `modifyParameters(bytes32 parameter`, `address addr)` - update an `address` parameter. Only present in the `RecyclingSurplusAuctionHouse`.
* `startAuction(amountToSell: uint256`, `initialBid: uint256)` - start a new surplus auction.
* `restartAuction(id: uint256)` - restart an auction if there have been 0 bids and the `auctionDeadline` has passed.
* `increaseBidSize(id: uint256`, `amountToBuy: uint256`, `bid: uint256)` - submit a bid with an increasing amount of protocol tokens for a fixed amount of system coins.
* `disableContract()` - disable the contract.
* `settleAuction(id: uint256)` - claim a winning bid / settles a completed auction.
* `terminateAuctionPrematurely(id: uint256)` - is used in case Governance wishes to upgrade (only) the `PreSettlementSurplusAuctionHouse` or in case `GlobalSettlement` is triggered. It settles `increaseBidSize` phase auctions, sending back the protocol tokens submitted by the `highBidder`.&#x20;

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
  * `bidAmount` - the amount of protocol tokens bid
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

## 3. Walkthrough <a href="#3-key-mechanisms-and-concepts" id="3-key-mechanisms-and-concepts"></a>

In a surplus auction, bidders compete for a fixed `amountToSell` of system coins with increasing `bidAmount`s of protocol tokens.

The surplus auction ends when the last bid's duration is passed (`bidDuration`) without another bid getting placed or when the auction duration (`totalAuctionLength`) has been surpassed. When the auction settles, the protocol tokens received are burnt in the case of a `BurningSurplusAuctionHouse` or transferred to a separate address in the case of `RecyclingSurplusAuctionHouse`.

In case governance disables the surplus auction house by calling `disableContract`, anyone can call `terminateAuctionPrematurely` in order to quickly settle an auction and return the protocol token bid to the `highBidder`.&#x20;

## 4. Gotchas (Potential source of user error)

#### **Keepers**

In the context of running a keeper (more info [here](https://github.com/reflexer-labs/geb-docs/tree/master/keepers)) in order to perform bids within an auction, a primary failure mode could occur when a keeper specifies an unprofitable price for FLX.

* This failure mode is due to the fact that there is nothing the system can do to stop a user from paying significantly more than the fair market value for the token in an auction (this goes for all auction types, `collateral`, `surplus`, and `debt`).
* Keepers that are performing badly in a `surplus` auction run the risk of overpaying FLX for the Coin as there is no upper limit to the `bidAmount` size other than their FLX balance.

#### **Bid Increments During an Auction**

During `increaseBidSize`, `bidAmount` amounts will increase by a `bidIncrease` percentage with each new `increaseBidSize`. The bidder must know the auction's `id`, specify the right amount of `amountToSell` for the auction, bid at least `bidIncrease` % more than the last bid and must have a sufficient FLX balance.

One risk is "front-running" or malicious miners. In this scenario, an honest keeper's bid of \[Past-bid + `bidIncrease`%] would get committed after the dishonest keeper's bid for the same, thereby preventing the honest keeper's bid from being accepted and forcing them to rebid with a higher price ((Past-bid + bidIncrease) + bidIncrease)). The dishonest keeper would need to pay higher gas fees to try to get a miner to put their transaction in first or collude with a miner to ensure their transaction is first. This could become especially important as the bid reaches the current market rate for FLX<>Coin.

**Quick** **Example**:

The `bidIncrease` could be set to 3%, meaning if the current bidder has placed a bid of 1 FLX, then the next bid must be at least 1.03 FLX. Overall, the purpose of the bid increment system is to incentivize early bidding and make the auction process move quickly.

#### Placing Bids Incorrectly

Bidders send FLX tokens from their addresses to the system/specific auction. If one bid is beat by another, the losing bid is refunded back to that bidder’s address. It’s important to note, however, that once a bid is submitted, there is no way to cancel it. The only possible way to have that bid returned is if it is outbid (or if the system goes into Global Settlement).

### **Illustration of the bidding flow:**

1. AccountingEngine `startAuction`'s a new Surplus Auction.
2. Bidder 1 sends a bid (FLX) that increases the `bidAmount` above the initial 0 value set during the `startAuction`. Bidder 1's FLX balance is decreased and the SurplusAuctionHouse's balance is increased by the bid size. `bid.highBidder` is reset from the AccountingEngine address to Bidder 1's and `bid.bidExpiry` is reset to `now + bidDuration`.
3. Next, Bidder 2 makes a bid that increases Bidder 1's bid by at least `bidIncrease`. Bidder 2's FLX balance is decreased and Bidder 1's balance is increased by Bidder 1's `bidAmount`. The difference between Bidder 2's and Bidder 1's `bidAmount` is sent from Bidder 2 to the SurplusAuctionHouse.
4. Bidder 1 then makes a bid that increases Bidder 2's `bidAmount` by at least `bidIncrease`. Bidder 1's FLX balance is decreased and Bidder 2's FLX balance is increased by Bidder 2's `bidAmount`. The amount Bidder 1 increased the bid is then sent from Bidder 1 to the SurplusAuctionHouse.
5. Bidder 2, as well as all the other bidders participating within the auction, decide it is no longer worth it to continue to bid higher `bidAmount`s, so they stop making bids. Once the `Bid.bidExpiry` expires, Bidder 1 calls `settleAuction` and the surplus Coin tokens are sent to the winning bidder's address (Bidder 1) in the `SafeEngine` and the system then burns the FLX received from the winning bidder. `gem.burn(address(this), bids[id].bid)`.

## 5. Failure Modes (Bounds on Operating Conditions & External Risk Factors)

* **Resulting from when FLX is burned**
  * There is the possibility where a situation arises where the FLX token makes the transaction revert (e.g. gets stopped or the AccountingEngine's permission to call burn() is revoked). In a case like this, deal can't succeed until someone fixes the issue with the FLX token. In the case of stoppage, this could include the deploying of a new FLX token. This new deployment could be completed by any individual using the MCD System but governance would need to add it to the system. Next, it would need to replace the old surplus and debt auctions with the new ones using the new FLX token. Lastly, it is crucial to enable the possibility to vote with the new version as well.
* **When there is massive surplus**
  * This would result in many SurplusAuctionHouse auctions occurring as the surplus over `surplusAuctionAmountToSell` + `surplusBuffer` is always auctioned off in `surplusAuctionAmountToSell` increments. However, auctions run concurrently, so this would "flood the keeper market" and possibly result in too few bids being placed on any auction. This could happen through keepers not bidding on multiple auctions at once, which would result in network congestion because all keepers are trying to bid on all of the auctions. This could also lead to possible keeper collusion (if the capital pool is large enough, they may be more willing to work together to split it evenly at the system's expense).
