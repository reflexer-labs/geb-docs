---
description: Shutting down GEB and returning collateral back to users
---

# Global Settlement

## 1. Summary <a href="#1-introduction-summary" id="1-introduction-summary"></a>

The `GlobalSettlement` is meant to shut down the system and reimburse SAFE users as well as system coin holders with all the collateral that's locked inside.

## 2. Contract Variables & Functions

**Variables**

* `authorizedAccounts[usr: address]`, `addAuthorization`/`removeAuthorization`/`isAuthorized` - auth mechanisms
* `safeEngine` - address of the `SAFEEngine`
* `liquidationEngine` - address of the `LiquidationEngine`
* `accountingEngine` - address of the `AccountingEngine`
* `osm` - address of the `OracleRelayer`
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

* `isAuthorized` **** - checks whether an address is part of `authorizedAddresses` (and thus can call authed functions).

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

### Global Settlement Properties Compared to MCD <a href="#current-implementation-properties-of-shutdown" id="current-implementation-properties-of-shutdown"></a>

**SAFE Redemption Prioritization**

We chose to give priority to SAFE users (when it comes to redeeming collateral) meaning that all users with positions above the `liquidationCRatio` (collateralization ratio under which a SAFE gets liquidated) should be allowed to retrieve surplus collateral.

### Settlement Stages <a href="#the-shutdown-mechanism-9-crucial-steps" id="the-shutdown-mechanism-9-crucial-steps"></a>

#### 1. `shutdownSystem()` : <a href="#1-cage" id="1-cage"></a>

* Freezes user entry-points
* Cancels collateral/surplus auctions
* Starts the cooldown period

#### 2. `freezeCollateralType(collateralType)` : <a href="#2-cage-collateralType" id="2-cage-collateralType"></a>

This step sets the final price for each `collateralType`, reading off the price feed. We must process some system state before it is possible to calculate the final systemCoin / collateral price. In particular, we need to determine:

a. `collateralShortfall` (considers under-collateralised SAFEs)

b. `outstandingCoinSupply` (after including system system surplus / deficit)

We determine (a) by processing all under-collateralised SAFEs with `processSAFE`.

#### 3. `processSAFE(collateralType, safe)` <a href="#3-skim-collateralType-urn" id="3-skim-collateralType-urn"></a>

This step cancels SAFE debt. Any excess collateral remains and the backing collateral is taken.

We determine the `outstandingCoinSupply` by processing ongoing coin generating processes, i.e. auctions. We need to ensure that auctions will not generate any further coin income. In the two-way auction model this occurs when all auctions are in the reverse (`decreaseSoldAmount`) phase. There are two ways of ensuring this: waiting until `shutdownCooldown` seconds have passed since the initial settlement trigger or use `fastTrackAuction`.

#### 4. `shutdownCooldown` or `fastTrackAuction` <a href="#4-wait-or-skip" id="4-wait-or-skip"></a>

1\. `shutdownCooldown`: set the cooldown period to be at least as long as the longest auction duration (which needs to be determined by the shutdown administrator). This takes a fairly predictable time to occur but with altered auction dynamics due to the now varying price of the system coin.&#x20;

2\. `fastTrackAuction`: cancel all ongoing auctions and seize the collateral. This allows for faster processing at the expense of more calls. This option allows coin holders to retrieve their collateral faster. Concretely, `fastTrackAuction(collateralType, auctionId)`:

* Cancels individual collateral auctions (using `terminateAuctionPrematurely`)
* Retrieves collateral to `GlobalSettlement` and (in case the system uses `EnglishCollateralHouse`s) returns coins (the bid) to the highest bidder

In the case of an `EnglishCollateralHouse`, `reduceAuctionedAmount` (reverse) phase auctions can continue normally.

Option (1) is sufficient for processing the system settlement but option (2) will speed up auctions. Both options are available in this implementation, with `fastTrackAuction` being enabled on a per-auction basis.

When a SAFE has been processed and has no debt remaining, the remaining collateral (SAFE surplus collateral above the liquidation ratio) can be removed.

#### 5. `freeCollateral(collateralType)` : <a href="#5-free-collateralType" id="5-free-collateralType"></a>

Removes collateral from the caller's SAFE.

After the processing period has elapsed, we enable calculation of the final price for each collateral type.

#### 6. `setOutstandingCoinSupply()` <a href="#6-thaw" id="6-thaw"></a>

This function is only callable after the processing time period has elapsed **and** if the `AccountingEngine` has no more surplus left. `AccountingEngine.transferPostSettlementSurplus` can be called in order to drain any remaining surplus and allow `setOutstandingCoinSupply` to be executed.

`setOutstandingCoinSupply` fixes the total outstanding supply of coins.

#### 7. `calculateCashPrice(collateralType)` <a href="#7-flow-collateralType" id="7-flow-collateralType"></a>

Calculates `collateralCashPrice` (amount of system coins per one unit of collateral). It adjusts the cash price in the case of system deficit / surplus.

At this point we have computed the final price for each collateral type and coin holders can now turn their coin into collateral. Each unit coin can claim a fixed basket of collateral.

Coin holders must first `prepareCoinsForRedeeming` into `coinBag`s. Once prepared, coins cannot be transferred out of the bag. More coins can be added to a bag later.

#### 8. `prepareCoinsForRedeeming(coinAmount)` <a href="#8-pack-wad" id="8-pack-wad"></a>

Put some coins into a `coinBag` in order to `redeemCollateral`. The bigger the bag, the more collateral the user can claim.

#### 9. `redeemCollateral(collateralType, collateralAmount)` <a href="#9-cash-collateralType-wad" id="9-cash-collateralType-wad"></a>

* Exchange some coins from a bag for a specific `collateralType`
* The amount of collateral available to redeem is limited by how big a bag is


## 4. Gotchas (Potential source of user error)

#### Keepers

We expect Keepers to buy up Coin from smallholders in order to claim collateral.

* This is because a majority of Coin holders are uncertain on how to do perform specific actions during the `GlobalSettlement` process. Due to this fact, we depend on third parties to buy up post-cage Coin to use for reclaiming large portions of Coin. Overall, there will be large amounts of Coin leftover in the system.

#### Note regarding `redeemCollateral`

At the end of the Global Settlement process, users will get a share of each collateral type. This will require them to call `redeemCollateral` through each collateralType in the system to completely cash out their Coin.

* **Example:** Users will need to call `redeemCollateral(collateralType, wad)` to redeem the proportional amount of the specified collateral that corresponds to the amount of Coin that was `prepareCoinsForRedeeming`’ ed, where the prepareCoinsForRedeeming function is used to aid with the redeeming of the different collaterals in different transactions. For example, let’s say you have 1000 Coin. You first prepareCoinsForRedeeming for the respective collateral types (`collateralTypes`), then for each `redeemCollateral` call, you will redeem what the 1000 Coin represents from the total Coin supply. In return, you will get the same proportion of that same collateral that was locked for all Coin holders. Therefore, the best approach a Coin holder can take is to `redeemCollateral` every collateral type (`collateralType`).
* An additional thing to note is that if any `collateralTypes` are undercollateralized, Coin holders will end up taking a bit of a cut as a result. This is because other `collateralTypes` will not be used to "cover for" an underwater collateral type.

#### DOS Attack

In order to prevent a DOS attack, whatever entity calls the `setOutstandingCoinSupply` function should ensure that `AccountingEngine.settleDebt()` is called within the same transaction.

* **Example:** An attacker can send small amounts of Coin in the `safeEngine` to the `accountingEngine`. This would prevent `setOutstandingCoinSupply` from being called and thus, GlobalSettlement from progressing. To prevent this, we would call `settleDebt` to clear out that excess Coin and proceed with `setOutstandingCoinSupply`.

#### Governance

It is important to set the correct `shutdownCooldown` period. If you set an incorrect `shutdownCooldown` period (if this is set early on) then auctions are later extended and this is not reset.

* It is important to note that the main problem to point out here is that if the `shutdownCooldown` allows `setOutstandingCoinSupply` to be called too early, all the `collateral` auctions may not have completed and the system may have an incorrect accounting of total `debt`.

#### **Other**

* The Cooldown period's purpose is so that auctions can be `fastTrackAuction`'d and `processSAFE`'d applies to all SAFES (not just the undercollateralized ones).
* Once the time period between global settlement and the cool-down period has passed, Coin holders are exposed to the ability to redeem their Coin for collateral.
  * Therefore the `shutdownCooldown` value should not be too large, so governance should advise for this at least.
* SAFE (processSAFE/GlobalSettlement) Keeper - is a tool to process underwater SAFEs if not all undercollateralized SAFES are accounted for. This Keeper could be used by GEB Stakeholders such as large Coin holders/custodians, FLX governors, Redemption keepers and more.

## 5. Failure Modes (Bounds on Operating Conditions & External Risk Factors)

Since `GlobalSettlement` will read the Collateral price from the `osm`, this can result in the collateral price being only as accurate as the last recorded price. If `osm` returns a bad price due to oracles getting hacked, the `GlobalSettlement` will be affected.

* **For example:** Calling Global Settlement because an oracle is getting attacked, we must make sure the oracles attack won’t affect the Global Settlement price because `GlobalSettlement` reads the price off of the `osm`.

If a bad price is queued up in the OSM, we need to make sure to fix the `finalCoinPerCollateralPrice` before the price is called on Global Settlement.

* **Example Scenario:** If a bad price goes through the `oracle`, it takes approximately 30 min for the OSM, and then the Global Settlement process takes over an hour to work. Therefore, by the time it triggers, you will have a bad price in the `osm` and this will cause the system to fail.
  * Saving this from happening depends on how quickly you react when it comes to an oracles attack as well as overall **governance** decisions.
  * **We do not believe Global Settlement is a viable solution to bad Oracles. They impact the system too quickly for Global Settlement to help.**

**An Oracle attack can be caused by two main events:**

* Low prices, which makes liquidations easy.
  * During Global Settlement, setting fake low prices would allow Coin holders to get too much collateral for their Coin, making this attack profitable.
* High prices, which helps with buying a lot of Coin.
  * When paired with a subsequent Global Settlement, this could be used to steal a lot / all of the collateral as that Coin would then be used to cash out.
    * **Example:** If a user is able to push up the price of a collateral type, it would allow them to mint a larger amount of Coin, resulting in a larger share of the Coin pool. Thus, they could claim a larger proportional share of the collateral whether it was of one type or a slice of all types. They could then readjust the manipulated prices before that collateral slice was fixed in the `GlobalSettlement`.

#### Critical Failure Modes

* `GlobalSettlement.withdraw` when set to maximum can result in it not being possible to call `setOutstandingCoinSupply` and therefore resulting in the GlobalSettlement not being able to proceed.
* `GlobalSettlement.withdraw`, when set to the minimum, can result in `setOutstandingCoinSupply` being called before all auctions have finished, resulting in debt being calculated incorrectly and ultimately setting a wrong collateral price.
* When `GlobalSettlement.shutdownSystem` is called, all Coin holders are left holding an unstable asset in place of their desired stable asset. This could result in a market price crash across all collateral due to liquidations & sell-offs.
GlobalSettlement.oracleRelayer - when set to attacker (`address`: set to attacker-controlled address), can cause global settlement to fail. This is unfixable. For this scenario to occur, the malicious entity (governance or otherwise) would need to be `auth`'ed on the `GlobalSettlement`.
