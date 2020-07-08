---
description: Shutting down GEB and returning collateral back to users
---

# Global Settlement

## 1. Summary <a id="1-introduction-summary"></a>

The `GlobalSettlement` is meant to shut down the system and reimburse CDP users as well as system coin holders.

## 2. Contract Variables & Functions

* `authorizedAccounts[usr: address]`, `addAuthorization`/`removeAuthorization`/`isAuthorized` - auth mechanisms
* `cdpEngine` - address of the `CDPEngine`
* `liquidationEngine` - address of the `LiquidationEngine`
* `accountingEngine` - address of the `AccountingEngine`
* `oracleRelayer` - address of the `OracleRelayer`
* `coinSavingsAccount` - address of the `CoinSavingsAccount`
* `rateSetter` - address of the Feedback Mechanism
* `stabilityFeeTreasury` - address of the `StabilityFeeTreasury`
* `contractEnabled` - whether settlement has been triggered or not
* `shutdownTime` - Unix timestamp of the moment when settlement was triggered
* `shutdownCooldown` -  the length of processing cooldown
* `outstandingCoinSupply` - outstanding system coin supply after all deficit / surplus has been taken into account
* `finalCoinPerCollateralPrice` - price per collateral type at time of settlement
* `collateralShortfall` - shortfall per collateral type \(taking into account under-collateralised CDPs\)
* `collateralTotalDebt` - outstanding debt per collateral type
* `collateralCashPrice` - amount of system coins to be paid for one unit of a specific collateral type
* `coinBag` - system coins ready to be swapped for collateral. Coins cannot be transferred out of a bag
* `coinsUsedToRedeem` - the amount of already swapped system coins for a specific address
* `modifyParameters` - update parameters such as the `cdpEngine`, `shutdownCooldown` etc
* `shutdownSystem()` - start the settlement process. Usually called by `ESM`
* `freezeCollateralType(bytes32 collateralType)` - sets the final price of a collateral type
* `fastTrackAuction(bytes32 collateralType, uint256 auctionId)` - cancel / terminate a live auction
* `processCDP(bytes32 collateralType, address cdp)` - cancel a CDP's owed debt 
* `freeCollateral(bytes32 collateralType)` - remove remaining collateral from a CDP \(can occur only after there's no debt in the CDP\)
* `setOutstandingCoinSupply()` - fix the outstanding supply of system coins
* `calculateCashPrice(bytes32 collateralType)` - calculates a price for each collateral type \(system coins per unit of collateral type\) taking into account system surplus / deficit
* `prepareCoinsForRedeeming(uint256 coinAmount)` - add system coins in a `coinBag` and prepare them to `redemmCollateral`
* `redeemCollateral(bytes32 collateralType, uint coinsAmount)` - swap coins from a bag with a specific collateral type

## 3. Walkthrough

### Global Settlement Properties Compared to MCD <a id="current-implementation-properties-of-shutdown"></a>

GEB has almost the same properties as MCD when it comes to settlement with the exception of one area: Accounting Engine \(former `Vow`\) Buffer Assistance. Currently, any post-settlement, leftover surplus is sent to the [`SettlementSurplusAuctioneer`](https://docs.reflexer.finance/system-contracts/auction-module/settlement-surplus-auctioner) and is automatically sold using the `PostSettlementSurplusAuctionHouse`.

We chose this strategy in order to avoid the edge case scenario where system coin holders are incentivized to trigger settlement only to redeem a disproportionate amount of collateral \(compared to CDP users\).

**CDP Redemption Prioritization**

We chose to give priority to CDP users \(when it comes to redeeming collateral\) meaning that all users with positions above the liquidation ratio \(collateralization ratio under which a CDP gets liquidated\) should be allowed to retrieve surplus collateral.

### Settlement Stages <a id="the-shutdown-mechanism-9-crucial-steps"></a>

#### 1. `shutdownSystem()` : <a id="1-cage"></a>

* Freezes user entry-points
* Cancels collateral/surplus auctions
* Starts the cooldown period

#### 2. `freezeCollateralType(collateralType)` : <a id="2-cage-ilk"></a>

This step sets the final price for each `collateralType`, reading off the price feed. We must process some system state before it is possible to calculate the final systemCoin / collateral price. In particular, we need to determine:

a. `collateralShortfall` \(considers under-collateralised CDPs\)

b. `outstandingCoinSupply` \(after including system system surplus / deficit\)

We determine \(a\) by processing all under-collateralised CDPs with `processCDP`.

#### 3. `processCDP(collateralType, cdp)` <a id="3-skim-ilk-urn"></a>

This step cancels CDP debt. Any excess collateral remains and the backing collateral is taken.

We determine the `outstandingCoinSupply` by processing ongoing coin generating processes, i.e. auctions. We need to ensure that auctions will not generate any further coin income. In the two-way auction model this occurs when all auctions are in the reverse \(`decreaseSoldAmount`\) phase. There are two ways of ensuring this: waiting until `shutdownCooldown` seconds have passed since the initial settlement trigger or use `fastTrackAuction`.

#### 4. `shutdownCooldown` or `fastTrackAuction` <a id="4-wait-or-skip"></a>

1. `shutdownCooldown`: set the cooldown period to be at least as long as the longest auction duration \(which needs to be determined by the shutdown administrator\). This takes a fairly predictable time to occur but with altered auction dynamics due to the now varying price of the system coin. 

2. `fastTrackAuction`: cancel all ongoing auctions and seize the collateral. This allows for faster processing at the expense of more calls. This option allows coin holders to retrieve their collateral faster. Concretely, `fastTrackAuction(collateralType, auctionId)`:

* Cancels individual collateral auctions that are in the `increaseBidSize` stage \(using `terminateAuctionPrematurely(auctionId)`\)
* Retrieves collateral to `GlobalSettlement` and returns coins \(the bid\) to the highest bidder

`reduceAuctionedAmount` \(reverse\) phase auctions can continue normally.

Option \(1\) is sufficient for processing the system settlement but option \(2\) will speed up auctions. Both options are available in this implementation, with `fastTrackAuction` being enabled on a per-auction basis.

When a CDP has been processed and has no debt remaining, the remaining collateral \(CDP surplus collateral above the liquidation ratio\) can be removed.

#### 5. `freeCollateral(collateralType)` : <a id="5-free-ilk"></a>

Removes collateral from the caller's CDP.

After the processing period has elapsed, we enable calculation of the final price for each collateral type.

#### 6. `setOutstandingCoinSupply()` <a id="6-thaw"></a>

This is only callable after the processing time period has elapsed. The assumption is that all under-collateralised CDPs have already been processed.

It fixes the total outstanding supply of coins \(it may also require extra CDP processing to cover system surplus aka drain surplus from the `AccountingEngine`\).

#### 7. `calculateCashPrice(collateralType)` <a id="7-flow-ilk"></a>

Calculates `collateralCashPrice` \(amount of system coins per one unit of collateral\). It adjusts the cash price in the case of system deficit / surplus.

At this point we have computed the final price for each collateral type and coin holders can now turn their coin into collateral. Each unit coin can claim a fixed basket of collateral.

Coin holders must first `prepareCoinsForRedeeming` into `coinBag`s. Once prepared, coins cannot be transferred out of the bag. More coins can be added to a bag later.

#### 8. `prepareCoinsForRedeeming(coinAmount)` <a id="8-pack-wad"></a>

Put some coins into a `coinBag` in order to `redeemCollateral`. The bigger the bag, the more collateral the user can claim.

#### 9. `redeemCollateral(collateralType, collateralAmount)` <a id="9-cash-ilk-wad"></a>

* Exchange some coins from a bag for a specific `collateralType`
* The amount of collateral available to redeem is limited by how big a bag is

