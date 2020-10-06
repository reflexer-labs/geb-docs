---
description: The post settlement surplus drain pipe
---

# Settlement Surplus Auctioneer

## 1. Summary <a id="1-introduction-summary"></a>

**Summary:** The `SettlementSurplusAuctioneer` is meant to trigger surplus auctions after `GlobalSettlement` has been activated and the `AccountingEngine` has been disabled. Any surplus amount that remains in the `AccountingEngine` post-settlement goes to system coin holders, in which case their expected payout \(amount of system collateral that can be redeemed\) is higher than it would have been in case there was no leftover surplus. In simple terms, system coin holders would start to value the coins at a higher price compared to the latest redemption price and would thus have an incentive to maliciously trigger settlement. In order to avoid this scenario, the `AccountingEngine` can send its surplus to the auctioneer which will then drain it using the `PostSettlementSurplusAuctionHouse`.

{% hint style="warning" %}
**Important Consideration**

The auctioneer is not an ideal solution for the situation where system coin holders are incentivized to trigger settlement. SAFE creators might take a haircut \(because of bad debt that could not be covered with post-settlement collateral/debt auctions\) when they have to redeem collateral, while the system will contract the protocol token's supply \(and thus favor protocol token holders over other actors\).

We are actively searching for an alternative that does not offer anyone an edge at the expense of other system participants. 
{% endhint %}

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `contractEnabled` - settlement flag \(can be `1` or `0`\)
* `authorizedAccounts[usr: address]` - addresses allowed to call `modifyParameters()` and `disableContract()`.
* `accountingEngine` - address pf the `AccountingEngine`.
* `surplusAuctionHouse` - address of the `PostSettlementSurplusAuctionHouse`.
* `safeEngine` - address of the SAFEEngine.
* `lastSurplusAuctionTime` - timestamp of the latest successful `auctionSurplus()` call.

**Modifiers**

* `isAuthorized` ****- checks whether an address is part of `authorizedAddresses` \(and thus can call authed functions\).

**Functions**

* `modifyParameters(parameter: bytes32`, `addr: address)` - change an address ****parameter.
* `disableContract()` - disable the auctioneer.
* `auctionSurplus()` - start a new post-settlement surplus auction.

**Events**

* `AddAuthorization` - emitted when a new address becomes authorized. Contains:
  * `account` - the new authorized account
* `RemoveAuthorization` - emitted when an address is de-authorized. Contains:
  * `account` - the address that was de-authorized
* `ModifyParameters` - emitted after a parameter is modified
* `AuctionSurplus` - emitted after a new post-settlement surplus auction starts. Contains:
  * `id` - the ID of the new post-settlement surplus auction house
  * `lastSurplusAuctionTime` - the current timestamp representing the last time when a surplus auction was triggered
  * `coinBalance` - the new auctioneer system coin balance

**Notes**

* Governance can only change the `accountingEngine` and the `surplusAuctionHouse` address using `modifyParameters`. The auctioneer can read all the other necessary parameters \(`surplusAuctionDelay`, `surplusAuctionAmountToSell`\) from the `accountingEngine`

  and use them when triggering post-settlement surplus auctions.

## 3. Walkthrough <a id="3-key-mechanisms-and-concepts"></a>

In case the auctioneer has surplus inside `SAFEEngine` \(`safeEngine.coinBalance[settlementSurplusAuctioneer]` &gt; 0\) anyone can call `SettlementSurplusAuctioneer.auctionSurplus` and start a new auction inside the `surplusAuctionHouse`. The auctioneer will use the same parameters for the surplus auction as the `AccountingEngine`. In fact, it will read all surplus auction related variables from the `AccountingEngine` before calling `surplusAuctionHouse`.

