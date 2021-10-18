---
description: Shutting down GEB and returning collateral back to users
---

# Global Settlement

## 1. Summary <a href="1-introduction-summary" id="1-introduction-summary"></a>

The `GlobalSettlement` is meant to shut down the system and reimburse SAFE users as well as system coin holders with all the collateral that's locked inside.

## 2. Contract Variables & Functions

**Variables**

* `authorizedAccounts[usr: address]`, `addAuthorization`/`removeAuthorization`/`isAuthorized` - auth mechanisms
* `safeEngine` - address of the `SAFEEngine`
* `liquidationEngine` - address of the `LiquidationEngine`
* `accountingEngine` - address of the `AccountingEngine`
* `oracleRelayer` - address of the `OracleRelayer`
* `coinSavingsAccount` - address of the `CoinSavingsAccount`
* `stabilityFeeTreasury` - address of the `StabilityFeeTreasury`
* `contractEnabled` - whether settlement has been triggered or not
* `shutdownTime` - unix timestamp of the moment when settlement was triggered
* `shutdownCooldown` -  cooldown post shutdown during which no on-chain processing can be done
* `outstandingCoinSupply` - outstanding system coin supply after all deficit / surplus has been taken into account
* `finalCoinPerCollateralPrice[collateralType: bytes32]` - price per collateral type at time of settlement
* `collateralShortfall[collateralType: bytes32]` - shortfall per collateral type (taking into account under-collateralised SAFEs)
* `collateralTotalDebt[collateralType: bytes32]` - outstanding debt per collateral type
* `collateralCashPrice[collateralType: bytes32]`- amount of system coins to be paid for one unit of a specific collateral type
* `coinBag[usr: address]` - system coins ready to be swapped for collateral. Coins cannot be transferred out of a bag
* `coinsUsedToRedeem[usr: address]` - the amount of already swapped system coins for a specific address

**Modifiers**

* `isAuthorized`** **- checks whether an address is part of `authorizedAddresses` (and thus can call authed functions).

**Functions**

* `modifyParameters()` - update parameters such as the `safeEngine`, `shutdownCooldown` etc
* `shutdownSystem()` - start the settlement process. Usually called by `ESM`
* `freezeCollateralType(collateralType: bytes32)` - sets the final price of a collateral type
* `fastTrackAuction(collateralType: bytes32, auctionId: uint256)` - cancel / terminate a live auction
* `processSAFE(collateralType: bytes32, safe: address)` - cancel a SAFE's owed debt&#x20;
* `freeCollateral(collateralType: bytes32)` - remove remaining collateral from a SAFE (can occur only after there's no debt in the SAFE)
* `setOutstandingCoinSupply()` - fix the outstanding supply of system coins
* `calculateCashPrice(collateralType: bytes32)` - calculates a price for each collateral type (system coins per unit of collateral type) taking into account system surplus / deficit
* `prepareCoinsForRedeeming(coinAmount: uint256)` - add system coins in a `coinBag` and prepare them to `redemmCollateral`
* `redeemCollateral(collateralType: bytes32, coinsAmount: uint)` - swap coins from a bag with a specific collateral type

## 3. Walkthrough

### Global Settlement Properties Compared to MCD <a href="current-implementation-properties-of-shutdown" id="current-implementation-properties-of-shutdown"></a>

**SAFE Redemption Prioritization**

We chose to give priority to SAFE users (when it comes to redeeming collateral) meaning that all users with positions above the `liquidationCRatio` (collateralization ratio under which a SAFE gets liquidated) should be allowed to retrieve surplus collateral.

### Settlement Stages <a href="the-shutdown-mechanism-9-crucial-steps" id="the-shutdown-mechanism-9-crucial-steps"></a>

#### 1. `shutdownSystem()` : <a href="1-cage" id="1-cage"></a>

* Freezes user entry-points
* Cancels collateral/surplus auctions
* Starts the cooldown period

#### 2. `freezeCollateralType(collateralType)` : <a href="2-cage-ilk" id="2-cage-ilk"></a>

This step sets the final price for each `collateralType`, reading off the price feed. We must process some system state before it is possible to calculate the final systemCoin / collateral price. In particular, we need to determine:

a. `collateralShortfall` (considers under-collateralised SAFEs)

b. `outstandingCoinSupply` (after including system system surplus / deficit)

We determine (a) by processing all under-collateralised SAFEs with `processSAFE`.

#### 3. `processSAFE(collateralType, safe)` <a href="3-skim-ilk-urn" id="3-skim-ilk-urn"></a>

This step cancels SAFE debt. Any excess collateral remains and the backing collateral is taken.

We determine the `outstandingCoinSupply` by processing ongoing coin generating processes, i.e. auctions. We need to ensure that auctions will not generate any further coin income. In the two-way auction model this occurs when all auctions are in the reverse (`decreaseSoldAmount`) phase. There are two ways of ensuring this: waiting until `shutdownCooldown` seconds have passed since the initial settlement trigger or use `fastTrackAuction`.

#### 4. `shutdownCooldown` or `fastTrackAuction` <a href="4-wait-or-skip" id="4-wait-or-skip"></a>

1\. `shutdownCooldown`: set the cooldown period to be at least as long as the longest auction duration (which needs to be determined by the shutdown administrator). This takes a fairly predictable time to occur but with altered auction dynamics due to the now varying price of the system coin.&#x20;

2\. `fastTrackAuction`: cancel all ongoing auctions and seize the collateral. This allows for faster processing at the expense of more calls. This option allows coin holders to retrieve their collateral faster. Concretely, `fastTrackAuction(collateralType, auctionId)`:

* Cancels individual collateral auctions (using `terminateAuctionPrematurely`)
* Retrieves collateral to `GlobalSettlement` and (in case the system uses `EnglishCollateralHouse`s) returns coins (the bid) to the highest bidder

In the case of an `EnglishCollateralHouse`, `reduceAuctionedAmount` (reverse) phase auctions can continue normally.

Option (1) is sufficient for processing the system settlement but option (2) will speed up auctions. Both options are available in this implementation, with `fastTrackAuction` being enabled on a per-auction basis.

When a SAFE has been processed and has no debt remaining, the remaining collateral (SAFE surplus collateral above the liquidation ratio) can be removed.

#### 5. `freeCollateral(collateralType)` : <a href="5-free-ilk" id="5-free-ilk"></a>

Removes collateral from the caller's SAFE.

After the processing period has elapsed, we enable calculation of the final price for each collateral type.

#### 6. `setOutstandingCoinSupply()` <a href="6-thaw" id="6-thaw"></a>

This function is only callable after the processing time period has elapsed **and** if the `AccountingEngine` has no more surplus left. `AccountingEngine.transferPostSettlementSurplus` can be called in order to drain any remaining surplus and allow `setOutstandingCoinSupply` to be executed.

`setOutstandingCoinSupply` fixes the total outstanding supply of coins.

#### 7. `calculateCashPrice(collateralType)` <a href="7-flow-ilk" id="7-flow-ilk"></a>

Calculates `collateralCashPrice` (amount of system coins per one unit of collateral). It adjusts the cash price in the case of system deficit / surplus.

At this point we have computed the final price for each collateral type and coin holders can now turn their coin into collateral. Each unit coin can claim a fixed basket of collateral.

Coin holders must first `prepareCoinsForRedeeming` into `coinBag`s. Once prepared, coins cannot be transferred out of the bag. More coins can be added to a bag later.

#### 8. `prepareCoinsForRedeeming(coinAmount)` <a href="8-pack-wad" id="8-pack-wad"></a>

Put some coins into a `coinBag` in order to `redeemCollateral`. The bigger the bag, the more collateral the user can claim.

#### 9. `redeemCollateral(collateralType, collateralAmount)` <a href="9-cash-ilk-wad" id="9-cash-ilk-wad"></a>

* Exchange some coins from a bag for a specific `collateralType`
* The amount of collateral available to redeem is limited by how big a bag is
