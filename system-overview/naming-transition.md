---
description: Going from cute to actual English words
---

# Core Contracts Naming Transition

The following tables show the before and after variable names from all core MCD contracts.

| Vat | CDPEngine |
| :--- | :--- |
| wards | authorizedAccounts |
| rely | addAuthorization |
| deny | removeAuthorization |
| auth | isAuthorized |
| can | cdpRights |
| hope | approveCDPModification |
| nope | denyCDPModification |
| wish | canModifyCDP |
| Ilk | CollateralType |
| Ilk.Art | CollateralType.debtAmount |
| Ilk.rate | CollateralType.accumulatedRates |
| Ilk.spot | CollateralType.safetyPrice |
| Ilk.line | CollateralType.debtCeiling |
| Ilk.dust | CollateralType.debtFloor |
| NaN | CollateralType.liquidationPrice \(NEW\) |
| Urn | CDP |
| Urn.ink | CDP.lockedCollateral |
| Urn.art | CDP.generatedDebt |
| ilks | collateralTypes |
| urns | cdps |
| gem | tokenCollateral |
| dai | coinBalance |
| sin | debtBalance |
| debt | globalDebt |
| vice | globalUnbackedDebt |
| Line | globalDebtCeiling |
| live | contractEnabled |
| note | emitLog |
| init | initializeCollateralType |
| file | modifyParameters |
| cage | disableContract |
| slip | modifyCollateralBalance |
| flux | transferCollateral |
| move | transferInternalCoins |
| frob | modifyCDPCollateralization |
| fork | transferCDPCollateralAndDebt |
| grab | confiscateCDPCollateralAndDebt |
| heal | settleDebt |
| suck | createUnbackedDebt |
| fold | updateAccumulatedRate |

| Vow | AccountingEngine |
| :--- | :--- |
| wards | authorizedAccounts |
| rely | addAuthorization |
| deny | removeAuthorization |
| auth | isAuthorized |
| vat | cdpEngine |
| flapper | surplusAuctionHouse |
| flopper | debtAuctionHouse |
| NaN | postSettlementSurplusDrain \(NEW\) |
| sin | debtQueue |
| NaN | activeDebtAuctions \(NEW\) |
| Sin | totalQueuedDebt |
| Ash | totalOnAuctionDebt |
| NaN | lastSurplusAuctionTime \(NEW\) |
| NaN | surplusAuctionDelay \(NEW\) |
| wait | popDebtDelay |
| dump | initialDebtAuctionMintedTokens |
| sump | debtAuctionBidSize |
| bump | surplusAuctionAmountToSell |
| hump | surplusBuffer |
| NaN | disableCooldown \(NEW\) |
| NaN | disableTimestamp \(NEW\) |
| NaN | protocolTokenAuthority \(NEW\) |
| live | contractEnabled |
| file | modifyParameters |
| fess | pushDebtToQueue |
| flog | popDebtFromQueue |
| heal | settleDebt |
| kiss | cancelAuctionedDebtWithSurplus |
| flop | auctionDebt |
| NaN | settleDebtAuction \(NEW\) |
| flap | auctionSurplus |
| cage | disableContract |
| NaN | transferPostSettlementSurplus \(NEW\) |

| Flap/per | SurplusAuctionHouse |
| :--- | :--- |
| wards | authorizedAccounts |
| rely | addAuthorization |
| deny | removeAuthorization |
| auth | isAuthorized |
| Bid | Bid |
| Bid.bid | Bid.bidAmount |
| Bid.lot | Bid.amountToSell |
| Bid.guy | Bid.highBidder |
| Bid.tic | Bid.bidExpiry |
| Bid.end | Bid.auctionDeadline |
| Kick | StartAuction |
| bids | bids |
| vat | cdpEngine |
| gem | protocolToken |
| beg | bidIncrease |
| ttl | bidDuration |
| tau | totalAuctionLength |
| kicks | auctionsStarted |
| live | contractEnabled |
| file | modifyParameters |
| kick | startAuction |
| tick | restartAuction |
| tend | increaseBidSize |
| deal | settleAuction |
| cage | disableContract |
| yank | terminateAuctionPrematurely |

| Flop/per | DebtAuctionHouse |
| :--- | :--- |
| wards | authorizedAccounts |
| rely | addAuthorization |
| deny | removeAuthorization |
| auth | isAuthorized |
| Bid | Bid |
| Bid.bid | Bid.bidAmount |
| Bid.lot | Bid.amountToSell |
| Bid.guy | Bid.highBidder |
| Bid.tic | Bid.bidExpiry |
| Bid.end | Bid.auctionDeadline |
| Kick | StartAuction |
| bids | bids |
| vat | cdpEngine |
| vow | accountingEngine |
| gem | protocolToken |
| beg | bidDecrease |
| pad | amountSoldIncrease |
| ttl | bidDuration |
| tau | totalAuctionLength |
| kicks | auctionsStarted |
| live | contractEnabled |
| file | modifyParameters |
| kick | startAuction |
| tick | restartAuction |
| dent | decreaseSoldAmount |
| deal | settleAuction |
| cage | disableContract |
| yank | terminateAuctionPrematurely |

| Flip/per | CollateralAuctionHouse |
| :--- | :--- |
| wards | authorizedAccounts |
| rely | addAuthorization |
| deny | removeAuthorization |
| auth | isAuthorized |
| Bid | Bid |
| Bid.bid | Bid.bidAmount |
| Bid.lot | Bid.amountToSell |
| Bid.guy | Bid.highBidder |
| Bid.tic | Bid.bidExpiry |
| Bid.end | Bid.auctionDeadline |
| Bid.usr | Bid.forgoneCollateralReceiver |
| Bid.gal | Bid.auctionIncomeRecipient |
| Bid.tab | Bid.amountToRaise |
| Kick | StartAuction |
| bids | bids |
| vat | cdpEngine |
| ilk | collateralType |
| beg | bidIncrease |
| ttl | bidDuration |
| tau | totalAuctionLength |
| kicks | auctionsStarted |
| cut | bidToMarketPriceRatio |
| spot | oracleRelayer |
| pip | orcl |
| Kick | StartAuction |
| live | contractEnabled |
| file | modifyParameters |
| kick | startAuction |
| tick | restartAuction |
| tend | increaseBidSize |
| dent | decreaseSoldAmount |
| deal | settleAuction |
| yank | terminateAuctionPrematurely |

| Join | BasicTokenAdapters |
| :--- | :--- |
| wards | authorizedAccounts |
| rely | addAuthorization |
| deny | removeAuthorization |
| auth | isAuthorized |
| GemLike | CollateralLike |
| GemJoin | CollateralJoin |
| vat | cdpEngine |
| ilk | collateralType |
| gem | collateral |
| dec | decimals |
| live | contractEnabled |
| cage | disableContract |
| join | join |
| exit | exit |
| ETHJoin | ETHJoin |
| DaiJoin | CoinJoin |
| dai | systemCoin |

| Cat | LiquidationEngine |
| :--- | :--- |
| wards | authorizedAccounts |
| rely | addAuthorization |
| deny | removeAuthorization |
| auth | isAuthorized |
| NaN | cdpSaviours \(NEW\) |
| NaN | connectCDPSaviour \(NEW\) |
| NaN | disconnectCDPSaviour \(NEW\) |
| Ilk | CollateralType |
| Ilk.flip | CollateralType.collateralAuctionHouse |
| Ilk.chop | CollateralType.liquidationPenalty |
| Ilk.lump | CollateralType.collateralToSell |
| ilks | collateralTypes |
| NaN | chosenCDPSaviour \(NEW\) |
| NaN | liquidationMutex \(NEW\) |
| live | contractEnabled |
| vat | cdpEngine |
| vow | accountingEngine |
| file | modifyParameters |
| flip | collateralAuctionHouse |
| cage | disableContract |
| NaN | protectCDP \(NEW\) |
| bite | liquidateCDP |
| urn | cdp |
| rate | accumulatedRates |
| ink | cdpCollateral |
| art | cdpDebt |
| lot | collateralToSell |
| grab | confiscateCDPCollateralAndDebt |
| fess | pushDebtToQueue |
| gal | initialBidder |
| tab | amountToRaise |
| bid | initialBid |
| Bite | Liquidate |
| NaN | SaveCDP \(NEW\) |

| Spot/ter | OracleRelayer |
| :--- | :--- |
| wards | authorizedAccounts |
| rely | addAuthorization |
| deny | removeAuthorization |
| auth | isAuthorized |
| Ilk | CollateralType |
| Ilk.pip | CollateralType.orcl |
| Ilk.mat | CollateralType.safetyCRatio |
| NaN | CollateralType.liquidationCRatio \(NEW\) |
| ilks | collateralTypes |
| vat | cdpEngine |
| par | redemptionPrice |
| NaN | redemptionPriceUpdateTime \(NEW\) |
| live | contractEnabled |
| Poke | UpdateCollateralPrice |
| file | modifyParameters |
| NaN | updateRedemptionPrice \(NEW\) |
| poke | updateCollateralPrice |
| cage | disableContract |

| Jug | TaxCollector |
| :--- | :--- |
| wards | authorizedAccounts |
| rely | addAuthorization |
| deny | removeAuthorization |
| auth | isAuthorized |
| Ilk | CollateralType |
| Ilk.duty | CollateralType.stabilityFee |
| Ilk.rho | CollateralType.updateTime |
| NaN | TaxReceiver \(NEW\) |
| NaN | TaxReceiver.canTakeBackTax \(NEW\) |
| NaN | TaxReceiver.taxPercentage \(NEW\) |
| ilks | collateralTypes |
| NaN | secondaryReceiverAllotedTax \(NEW\) |
| NaN | usedSecondaryReceiver \(NEW\) |
| NaN | secondaryReceiverAccounts \(NEW\) |
| NaN | secondaryReceiverRevenueSources \(NEW\) |
| NaN | secondaryTaxReceivers \(NEW\) |
| vat | cdpEngine |
| vow | primaryTaxReceiver |
| base | globalStabilityFee |
| NaN | secondaryReceiverNonce \(NEW\) |
| NaN | maxSecondaryReceivers \(NEW\) |
| NaN | latestSecondaryReceiver \(NEW\) |
| NaN | collateralList \(NEW\) |
| NaN | secondaryReceiverList \(NEW\) |
| init | initializeCollateralType |
| file | modifyParameters |
| NaN | addSecondaryReceiver \(NEW\) |
| NaN | modifySecondaryReceiver \(NEW\) |
| NaN | collectedAllTax \(NEW\) |
| NaN | taxManyOutcome \(NEW\) |
| NaN | secondaryReceiversAmount \(NEW\) |
| NaN | isSecondaryReceiver \(NEW\) |
| NaN | collateralListLength \(NEW\) |
| NaN | taxSingleOutcome \(NEW\) |
| drip | taxMany \(NEW\) / taxSingle |
| NaN | splitTaxIncome \(NEW\) |
| NaN | distributeTax \(NEW\) |
| NaN | CollectTax \(NEW\) |
| NaN | DistributeTax \(NEW\) |

| Pot | CoinSavingsAccount |
| :--- | :--- |
| wards | authorizedAccounts |
| rely | addAuthorization |
| deny | removeAuthorization |
| auth | isAuthorized |
| pie | savings |
| Pie | totalSavings |
| dsr | savingsRate |
| chi | accumulatedRates |
| vat | cdpEngine |
| file | modifyParameters |
| cage | disableContract |
| drip | updateAccumulatedRate |
| NaN | nextAccumulatedRate \(NEW\) |
| join | deposit |
| exit | withdraw |

| End | GlobalSettlement |
| :--- | :--- |
| wards | authorizedAccounts |
| rely | addAuthorization |
| deny | removeAuthorization |
| auth | isAuthorized |
| vat | cdpEngine |
| cat | liquidationEngine |
| vow | accountingEngine |
| spot | oracleRelayer |
| pot | coinSavingsAccount |
| NaN | rateSetter \(NEW\) |
| NaN | stabilityFeeTreasury \(NEW\) |
| live | contractEnabled |
| when | shutdownTime |
| wait | shutdownCooldown |
| debt | outstandingCoinSupply |
| tag | finalCoinPerCollateralPrice |
| gap | collateralShortfall |
| Art | collateralTotalDebt |
| fix | collateralCashPrice |
| bag | coinBag |
| out | coinsUsedToRedeem |
| file | modifyParameters |
| cage | shutdownSystem / freezeCollateralType |
| skip | fastTrackAuction |
| skim | processCDP |
| urn | cdp |
| owe | amountOwed |
| free | freeCollateral |
| thaw | setOutstandingCoinSupply |
| flow | calculateCashPrice |
| pack | prepareCoinsForRedeeming |
| cash | redeemCollateral |

| Dai | Coin |
| :--- | :--- |
| wards | authorizedAccounts |
| rely | addAuthorization |
| deny | removeAuthorization |
| auth | isAuthorized |

| Lib | Logging |
| :--- | :--- |
| note | emitLog |

