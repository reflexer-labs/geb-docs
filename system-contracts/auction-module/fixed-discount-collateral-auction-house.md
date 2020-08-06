---
description: >-
  Auctioneer that tries to recapitalize the system by selling collateral at a
  discounted price
---

# Fixed Discount Collateral Auction House

## 1. Summary <a id="1-introduction-summary"></a>

Fixed discount collateral auctions are similar to their English counterpart in that they are used to preserve the overall system health by liquidating undercollateralized CDPs. This collateral type automatically calculates an amount of collateral to send back to a bidder according to the amount of system coins the bidder submits as well as to the current system coin `redemptionPrice` and collateral market price.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `contractEnabled` - settlement flag \(`1` or `0`\).
* `AUCTION_HOUSE_TYPE` - flag set to `bytes32("COLLATERAL")`
* `AUCTION_TYPE` - flag set to `bytes32("FIXED_DISCOUNT")`.
* `authorizedAccounts[usr: address]` - addresses allowed to call `modifyParameters()` and `disableContract()`.
* `cdpEngine` - storage of the `CDPEngine`'s address.
* `bids[id: uint]` - storage of all bids.
* `collateralType` - id of the collateral type for which the `CollateralAuctionHouse` is responsible.
* `totalAuctionLength` - auction length \(default: 7 days\).
* `auctionsStarted` - total auction count, used to track auction `id`s.
* `oracleRelayer` - address of the `OracleRelayer`.
* `osm` - collateral type `OSM` address.
* `median` - collateral type medianizer address.

## 3. Walkthrough <a id="3-key-mechanisms-and-concepts"></a>

