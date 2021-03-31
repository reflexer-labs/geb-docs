---
description: Going from cute to actual English words
---

# Core Contracts Naming Transition

The following tables show the before and after variable names from all core MCD contracts.

| MCD Units | Meaning |
| :--- | :--- |
| WAD | Number with 18 decimals \(10^18\) |
| RAY | Number with 27 decimals \(10^27\) |
| RAD | Number with 45 decimals \(10^45\) |

| Vat | SAFEEngine |
| :--- | :--- |
| wards | authorizedAccounts |
| rely | addAuthorization |
| deny | removeAuthorization |
| auth | isAuthorized |
| hope | approveSAFEModification |
| nope | denySAFEModification |
| wish | canModifySAFE |
| Ilk | CollateralType |
| Ilk.Art | CollateralType.debtAmount |
| Ilk.rate | CollateralType.accumulatedRate |
| Ilk.spot | CollateralType.safetyPrice |
| Ilk.line | CollateralType.debtCeiling |
| Ilk.dust | CollateralType.debtFloor |
| NaN | CollateralType.liquidationPrice \(NEW\) |
| Urn | SAFE |
| Urn.ink | SAFE.lockedCollateral |
| Urn.art | SAFE.generatedDebt |
| ilks | collateralTypes |
| urns | safes |
| gem | tokenCollateral |
| dai | coinBalance |
| sin | debtBalance |
| debt | globalDebt |
| vice | globalUnbackedDebt |
| Line | globalDebtCeiling |
| live | contractEnabled |
| init | initializeCollateralType |
| file | modifyParameters |
| cage | disableContract |
| slip | modifyCollateralBalance |
| flux | transferCollateral |
| move | transferInternalCoins |
| frob | modifySAFECollateralization |
| dink | deltaCollateral |
| dart | deltaDebt |
| fork | transferSAFECollateralAndDebt |
| grab | confiscateSAFECollateralAndDebt |
| heal | settleDebt |
| suck | createUnbackedDebt |
| fold | updateAccumulatedRate |
| NaN | AddAuthorization \(NEW\) |
| NaN | RemoveAuthorization \(NEW\) |
| NaN | ApproveSAFEModification \(NEW\) |
| NaN | DenySAFEModification \(NEW\) |
| NaN | InitializeCollateralType \(NEW\) |
| NaN | ModifyParameters \(NEW\) |
| NaN | ModifyParameters \(NEW\) |
| NaN | DisableContract \(NEW\) |
| NaN | ModifyCollateralBalance \(NEW\) |
| NaN | TransferCollateral \(NEW\) |
| NaN | TransferInternalCoins \(NEW\) |
| NaN | ModifySAFECollateralization \(NEW\) |
| NaN | TransferSAFECollateralAndDebt \(NEW\) |
| NaN | ConfiscateSAFECollateralAndDebt \(NEW\) |
| NaN | SettleDebt \(NEW\) |
| NaN | CreateUnbackedDebt \(NEW\) |
| NaN | UpdateAccumulatedRate \(NEW\) |

| Vow | AccountingEngine |
| :--- | :--- |
| wards | authorizedAccounts |
| rely | addAuthorization |
| deny | removeAuthorization |
| auth | isAuthorized |
| vat | safeEngine |
| flapper | surplusAuctionHouse |
| flopper | debtAuctionHouse |
| NaN | postSettlementSurplusDrain \(NEW\) |
| NaN | extraSurplusReceiver \(NEW\) |
| sin | debtQueue |
| NaN | debtPoppers \(NEW\) |
| Sin | totalQueuedDebt |
| Ash | totalOnAuctionDebt |
| NaN | lastSurplusAuctionTime \(NEW\) |
| NaN | lastSurplusTransferTime \(NEW\) |
| NaN | surplusAuctionDelay \(NEW\) |
| NaN | surplusTransferDelay \(NEW\) |
| NaN | extraSurplusIsTransferred \(NEW\) |
| wait | popDebtDelay |
| dump | initialDebtAuctionMintedTokens |
| sump | debtAuctionBidSize |
| bump | surplusAuctionAmountToSell |
| NaN | surplusTransferAmount \(NEW\) |
| hump | surplusBuffer |
| NaN | disableCooldown \(NEW\) |
| NaN | disableTimestamp \(NEW\) |
| NaN | protocolTokenAuthority \(NEW\) |
| NaN | systemStakingPool \(NEW\) |
| live | contractEnabled |
| NaN | unqueuedUnauctionedDebt |
| file | modifyParameters |
| fess | pushDebtToQueue |
| flog | popDebtFromQueue |
| heal | settleDebt |
| kiss | cancelAuctionedDebtWithSurplus |
| flop | auctionDebt |
| NaN | settleDebtAuction \(NEW\) |
| flap | auctionSurplus |
| NaN | transferExtraSurplus \(NEW\) |
| cage | disableContract |
| NaN | transferPostSettlementSurplus \(NEW\) |
| NaN | AddAuthorization \(NEW\) |
| NaN | RemoveAuthorization \(NEW\) |
| NaN | ModifyParameters \(NEW\) |
| NaN | ModifyParameters \(NEW\) |
| NaN | PushDebtToQueue \(NEW\) |
| NaN | PopDebtFromQueue \(NEW\) |
| NaN | SettleDebt \(NEW\) |
| NaN | CancelAuctionedDebtWithSurplus \(NEW\) |
| NaN | AuctionDebt \(NEW\) |
| NaN | AuctionSurplus \(NEW\) |
| NaN | DisableContract \(NEW\) |
| NaN | TransferPostSettlementSurplus \(NEW\) |
| NaN | TransferExtraSurplus \(NEW\) |

| Flap/per | Burning/Recycling/PostSettlementSurplusAuctionHouse |
| :--- | :--- |
| wards | authorizedAccounts |
| rely | addAuthorization |
| deny | removeAuthorization |
| auth | isAuthorized |
| NaN | AUCTION\_HOUSE\_TYPE \(NEW\) |
| Bid | Bid |
| Bid.bid | Bid.bidAmount |
| Bid.lot | Bid.amountToSell |
| Bid.guy | Bid.highBidder |
| Bid.tic | Bid.bidExpiry |
| Bid.end | Bid.auctionDeadline |
| Kick | StartAuction |
| bids | bids |
| vat | safeEngine |
| gem | protocolToken |
| NaN | protocolTokenBidReceiver \(only in the Recycling version\) |
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
| yank | terminateAuctionPrematurely \(only in the Burning & Recycling versions\) |
| NaN | AddAuthorization \(NEW\) |
| NaN | RemoveAuthorization \(NEW\) |
| NaN | ModifyParameters \(NEW\) |
| NaN | RestartAuction \(NEW\) |
| NaN | IncreaseBidSize \(NEW\) |
| NaN | StartAuction \(NEW\) |
| NaN | SettleAuction \(NEW\) |
| NaN | DisableContract \(NEW\) |
| NaN | TerminateAuctionPrematurely \(NEW\) |

| Flop/per | DebtAuctionHouse |
| :--- | :--- |
| wards | authorizedAccounts |
| rely | addAuthorization |
| deny | removeAuthorization |
| auth | isAuthorized |
| NaN | AUCTION\_HOUSE\_TYPE |
| Bid | Bid |
| Bid.bid | Bid.bidAmount |
| Bid.lot | Bid.amountToSell |
| Bid.guy | Bid.highBidder |
| Bid.tic | Bid.bidExpiry |
| Bid.end | Bid.auctionDeadline |
| Kick | StartAuction |
| bids | bids |
| vat | safeEngine |
| vow | accountingEngine |
| gem | protocolToken |
| beg | bidDecrease |
| pad | amountSoldIncrease |
| ttl | bidDuration |
| tau | totalAuctionLength |
| kicks | auctionsStarted |
| NaN | activeDebtAuctions |
| live | contractEnabled |
| file | modifyParameters |
| kick | startAuction |
| tick | restartAuction |
| dent | decreaseSoldAmount |
| deal | settleAuction |
| cage | disableContract |
| yank | terminateAuctionPrematurely |
| NaN | AddAuthorization \(NEW\) |
| NaN | RemoveAuthorization \(NEW\) |
| NaN | StartAuction \(NEW\) |
| NaN | ModifyParameters \(NEW\) |
| NaN | RestartAuction \(NEW\) |
| NaN | DecreaseSoldAmount \(NEW\) |
| NaN | SettleAuction \(NEW\) |
| NaN | TerminateAuctionPrematurely \(NEW\) |
| NaN | DisableContract \(NEW\) |

| Flip/per | English/Fixed/IncreasingDiscountCollateralAuctionHouse |
| :--- | :--- |
| wards | authorizedAccounts |
| rely | addAuthorization |
| deny | removeAuthorization |
| auth | isAuthorized |
| NaN | AUCTION\_HOUSE\_TYPE \(NEW\) |
| NaN | AUCTION\_TYPE \(NEW\) |
| Bid | Bid |
| NaN | raisedAmount \(NEW\) \(only in the FixedDiscount version\) |
| NaN | soldAmount\(NEW\) \(only in the FixedDiscount version\) |
| Bid.bid | Bid.bidAmount \(only in the English version\) |
| Bid.lot | Bid.amountToSell |
| Bid.guy | Bid.highBidder \(only in the English version\) |
| Bid.tic | Bid.bidExpiry \(only in the English version\) |
| Bid.end | Bid.auctionDeadline |
| Bid.usr | Bid.forgoneCollateralReceiver |
| Bid.gal | Bid.auctionIncomeRecipient |
| Bid.tab | Bid.amountToRaise |
| NaN | Bid.currentDiscount \(NEW\) \(only in the IncreasingDiscount version\) |
| NaN | Bid.maxDiscount \(NEW\) \(only in the IncreasingDiscount version\) |
| NaN | Bid.perSecondDiscountUpdateRate \(NEW\) \(only in the IncreasingDiscount version\) |
| NaN | Bid.discountIncreaseDeadline \(NEW\) \(only in the IncreasingDiscount version\) |
| Kick | StartAuction |
| bids | bids |
| vat | safeEngine |
| ilk | collateralType |
| NaN | lastReadRedemptionPrice \(only in the FixedDiscount version\) |
| NaN | minimumBid \(NEW\) \(only in the FixedDiscount version\) |
| beg | bidIncrease \(only in the English version\) |
| ttl | bidDuration \(only in the English version\) |
| tau | totalAuctionLength |
| kicks | auctionsStarted |
| NaN | discount \(NEW\) \(only in the FixedDiscount version\) |
| NaN | minDiscount \(NEW\) \(only in the IncreasingDiscount version\) |
| NaN | maxDiscount \(NEW\) \(only in the IncreasingDiscount version\) |
| NaN | perSecondDiscountUpdateRate \(NEW\) \(only in the IncreasingDiscount version\) |
| NaN | maxDiscountUpdateRateTimeline \(NEW\) \(only in the IncreasingDiscount version\) |
| NaN | lowerCollateralMedianDeviation \(NEW\) \(only in the FixedDiscount version\) |
| NaN | upperCollateralMedianDeviation \(NEW\) \(only in the FixedDiscount version\) |
| NaN | lowerSystemCoinMedianDeviation \(NEW\) \(only in the FixedDiscount version\) |
| NaN | upperSystemCoinMedianDeviation \(NEW\) \(only in the FixedDiscount version\) |
| NaN | minSystemCoinMedianDeviation \(NEW\) \(only in the FixedDiscount version\) |
| cut | bidToMarketPriceRatio \(only in the English version\) |
| spot | oracleRelayer |
| pip | orcl/osm |
| NaN | median \(NEW\) \(only in the FixedDiscount version\) |
| Kick | StartAuction |
| live | contractEnabled |
| file | modifyParameters |
| NaN | getDiscountedCollateralPrice \(NEW\) \(only in the Fixed and Increasing Discount versions\) |
| NaN | getBoughtCollateral \(NEW\) \(only in the Fixed and Increasing Discount versions\) |
| NaN | getSystemCoinFloorDeviatedPrice \(NEW\) \(only in the Fixed and Increasing Discount versions\) |
| NaN | getSystemCoinCeilingDeviatedPrice \(NEW\) \(only in the Fixed and Increasing Discount versions\) |
| NaN | getCollateralFSMAndFinalSystemCoinPrices \(NEW\) \(only in the Fixed and Increasing Discount versions\) |
| NaN | getFinalBaseCollateralPrice \(NEW\) \(only in the Fixed and Increasing Discount versions\) |
| NaN | getNextCurrentDiscount \(NEW\) \(only in the IncreasingDiscount version\) |
| NaN | getCollateralBought \(NEW\) \(only in the FixedDiscount version\) |
| NaN | buyCollateral \(NEW\) \(only in the FixedDiscount version\) |
| kick | startAuction |
| tick | restartAuction \(only in the English version\) |
| tend | increaseBidSize \(only in the English version\) |
| dent | decreaseSoldAmount \(only in the English version\) |
| deal | settleAuction |
| yank | terminateAuctionPrematurely |
| NaN | bidAmount \(NEW\) |
| NaN | remainingAmountToSell \(NEW\) |
| NaN | forgoneCollateralReceiver \(NEW\) |
| NaN | amountToRaise \(NEW\) |
| NaN | AddAuthorization \(NEW\) |
| NaN | RemoveAuthorization \(NEW\) |
| NaN | StartAuction \(NEW\) |
| NaN | ModifyParameters \(NEW\) |
| NaN | BuyCollateral \(NEW\) |
| NaN | SettleAuction \(NEW\) |
| NaN | TerminateAuctionPrematurely \(NEW\) |

| Join | BasicTokenAdapters |
| :--- | :--- |
| wards | authorizedAccounts |
| rely | addAuthorization |
| deny | removeAuthorization |
| auth | isAuthorized |
| GemLike | CollateralLike |
| GemJoin | CollateralJoin |
| vat | safeEngine |
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
| NaN | AddAuthorization \(NEW\) |
| NaN | RemoveAuthorization \(NEW\) |
| NaN | DisableContract \(NEW\) |
| NaN | Join \(NEW\) |
| NaN | Exit \(NEW\) |

| Cat | LiquidationEngine |
| :--- | :--- |
| wards | authorizedAccounts |
| rely | addAuthorization |
| deny | removeAuthorization |
| auth | isAuthorized |
| NaN | safeSaviours \(NEW\) |
| NaN | connectSAFESaviour \(NEW\) |
| NaN | disconnectSAFESaviour \(NEW\) |
| Ilk | CollateralType |
| Ilk.flip | CollateralType.collateralAuctionHouse |
| Ilk.chop | CollateralType.liquidationPenalty |
| Ilk.dunk | CollateralType.liquidationQuantity |
| box | onAuctionSystemCoinLimit |
| litter | currentOnAuctionSystemCoins |
| ilks | collateralTypes |
| NaN | chosenSAFESaviour \(NEW\) |
| NaN | mutex \(NEW\) |
| live | contractEnabled |
| vat | safeEngine |
| vow | accountingEngine |
| file | modifyParameters |
| flip | collateralAuctionHouse |
| cage | disableContract |
| NaN | protectSAFE \(NEW\) |
| bite | liquidateSAFE |
| claw | removeCoinsFromAuction |
| NaN | getLimitAdjustedDebtToCover \(NEW\) |
| urn | safe |
| rate | accumulatedRates |
| ink | safeCollateral |
| art | safeDebt |
| dust | debtFloor |
| grab | confiscateSAFECollateralAndDebt |
| fess | pushDebtToQueue |
| mink | collateralData |
| gal | initialBidder |
| tab | amountToRaise |
| bid | initialBid |
| Bite | Liquidate |
| NaN | AddAuthorization \(NEW\) |
| NaN | RemoveAuthorization \(NEW\) |
| NaN | ConnectSAFESaviour \(NEW\) |
| NaN | DisconnectSAFESaviour \(NEW\) |
| NaN | UpdateCurrentOnAuctionSystemCoins \(NEW\) |
| NaN | ModifyParameters \(NEW\) |
| NaN | DisableContract \(NEW\) |
| NaN | SaveSAFE \(NEW\) |
| NaN | FailedSAFESave \(NEW\) |
| NaN | ProtectSAFE \(NEW\) |

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
| vat | safeEngine |
| par | redemptionPrice |
| NaN | redemptionPriceUpdateTime \(NEW\) |
| NaN | redemptionRateUpperBound \(NEW\) |
| NaN | redemptionRateLowerBound \(NEW\) |
| live | contractEnabled |
| Poke | UpdateCollateralPrice |
| file | modifyParameters |
| NaN | updateRedemptionPrice \(NEW\) |
| poke | updateCollateralPrice |
| cage | disableContract |
| NaN | AddAuthorization \(NEW\) |
| NaN | RemoveAuthorization \(NEW\) |
| NaN | DisableContract \(NEW\) |
| NaN | ModifyParameters \(NEW\) |
| NaN | UpdateRedemptionPrice \(NEW\) |
| NaN | UpdateCollateralPrice \(NEW\) |

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
| vat | safeEngine |
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
| NaN | AddAuthorization \(NEW\) |
| NaN | RemoveAuthorization \(NEW\) |
| NaN | InitializeCollateralType \(NEW\) |
| NaN | ModifyParameters \(NEW\) |
| NaN | AddSecondaryReceiver \(NEW\) |
| NaN | ModifySecondaryReceiver \(NEW\) |
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
| vat | safeEngine |
| file | modifyParameters |
| cage | disableContract |
| drip | updateAccumulatedRate |
| NaN | nextAccumulatedRate \(NEW\) |
| join | deposit |
| exit | withdraw |
| NaN | AddAuthorization \(NEW\) |
| NaN | RemoveAuthorization \(NEW\) |
| NaN | RemoveAuthorization \(NEW\) |
| NaN | DisableContract \(NEW\) |
| NaN | Deposit \(NEW\) |
| NaN | Withdraw \(NEW\) |
| NaN | UpdateAccumulatedRate \(NEW\) |

| End | GlobalSettlement |
| :--- | :--- |
| wards | authorizedAccounts |
| rely | addAuthorization |
| deny | removeAuthorization |
| auth | isAuthorized |
| vat | safeEngine |
| cat | liquidationEngine |
| vow | accountingEngine |
| spot | oracleRelayer |
| pot | coinSavingsAccount |
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
| skim | processSAFE |
| urn | safe |
| owe | amountOwed |
| free | freeCollateral |
| thaw | setOutstandingCoinSupply |
| flow | calculateCashPrice |
| pack | prepareCoinsForRedeeming |
| cash | redeemCollateral |
| NaN | AddAuthorization \(NEW\) |
| NaN | RemoveAuthorization \(NEW\) |
| NaN | ModifyParameters \(NEW\) |
| NaN | ShutdownSystem \(NEW\) |
| NaN | FreezeCollateralType \(NEW\) |
| NaN | FastTrackAuction \(NEW\) |
| NaN | ProcessSAFE \(NEW\) |
| NaN | FreeCollateral \(NEW\) |
| NaN | SetOutstandingCoinSupply \(NEW\) |
| NaN | CalculateCashPrice \(NEW\) |
| NaN | PrepareCoinsForRedeeming \(NEW\) |
| NaN | RedeemCollateral \(NEW\) |

| Dai | Coin |
| :--- | :--- |
| wards | authorizedAccounts |
| rely | addAuthorization |
| deny | removeAuthorization |
| auth | isAuthorized |
| name | name |
| symbol | symbol |
| version | version |
| decimals | decimals |
| totalSupply | totalSupply |
| balanceOf | balanceOf |
| allowance | allowance |
| nonces | nonces |
| NaN | changeData \(NEW\) |
| Approval | Approval |
| Transfer | Transfer |
| DOMAIN\_SEPARATOR | DOMAIN\_SEPARATOR |
| PERMIT\_TYPEHASH | PERMIT\_TYPEHASH |
| transfer | transfer |
| transferFrom | transferFrom |
| mint | mint |
| burn | burn |
| approve | approve |
| push | push |
| pull | pull |
| move | move |
| permit | permit |

