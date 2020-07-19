---
description: The post settlement surplus drain pipe
---

# Settlement Surplus Auctioneer

## 1. Summary <a id="1-introduction-summary"></a>

**Summary:** The `SettlementSurplusAuctioneer` is meant to trigger surplus auctions after `GlobalSettlement` has been activated and the `AccountingEngine` has been disabled. Any surplus amount that remains in the `AccountingEngine` post-settlement goes to system coin holders, in which case their expected payout \(amount of system collateral that can be redeemed\) is higher than it would have been in case there was no leftover surplus. In simple terms, system coin holders would start to value the coins at a higher price compared to the latest redemption price and would thus have an incentive to maliciously trigger settlement. In order to avoid this scenario, the `AccountingEngine` can send its surplus to the auctioneer which will then drain it using the `SurplusAuctionHouse`.

{% hint style="warning" %}
**Important Consideration**

The auctioneer is not an ideal solution for the situation where system coin holders are incentivized to trigger settlement. CDP creators might take a haircut \(because of bad debt that could not be covered with collateral auctions post-settlement\) when they have to redeem collateral, while the system will contract the protocol token's supply \(and thus favor protocol token holders over other actors\).

We are actively searching for an alternative that does not offer anyone an edge at the expense of other system participants. 
{% endhint %}

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

* `authorizedAccounts [usr: address]` - `addAuthorization`/`removeAuthorization`/`isAuthorized` - auth mechanisms
* `accountingEngine` - address of the `AccountingEngine`
* `surplusAuctionHouse` - address of the `PostSettlementSurplusAuctionHouse`
* `cdpEngine` - address of the `CDPEngine`
* `contractEnabled` - settlement flag
* `lastSurplusAuctionTime` - last timestamp when the auctioneer started a surplus auction
* `modifyParameters` - change a parameter in the auctioneer
* `disableContract` - disable the auctioneer
* `auctionSurplus` - start a new surplus auction

### **Parameters Set By Governance** <a id="parameters-set-by-governance"></a>

 Governance can only change the `accountingEngine` address using `modifyParameters`. The auctioneer can read all the other necessary parameters \(`surplusAuctionDelay`, `surplusAuctionAmountToSell`, `surplusAuctionHouse`\) from the `AccountingEngine` and use them when triggering surplus auctions.

## 3. Walkthrough <a id="3-key-mechanisms-and-concepts"></a>

In case the auctioneer has surplus inside `CDPEngine` \(`cdpEngine.coinBalance[settlementSurplusAuctioneer]` &gt; 0\) anyone can call `SettlementSurplusAuctioneer.auctionSurplus` and start a new auction inside the `PostSettlementSurplusAuctionHouse`. The auctioneer will use the same parameters for the surplus auction as the `AccountingEngine`. In fact, it will read all surplus auction related variables from the `AccountingEngine` before calling the auction contract.

