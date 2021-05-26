# Current System Parameters

## Mainnet RAI

### Quick Glance

* Minimum amount of debt per Safe: 800
* Maximum amount of debt per Safe: 10B
* ETH-A annual borrow rate/stability fee: 2%
* Global debt ceiling: MAX\_UINT
* Liquidation penalty: 12%
* ETH-A auction fixed discount: 10% 
* ETH-A fixed discount auction minimum bid: 25 \(WAD\)

### AccountingEngine

* `debtAuctionBidSize` - 35,000 \(RAD\)
* `disableCooldown` - 1,814,400 \(21 days\)
* `initialDebtAuctionMintedTokens` - 439 \(WAD\)
* `popDebtDelay` - 1,036,800 \(12 days\)
* `surplusAuctionAmountToSell` - 6,000 \(RAD\)
* `surplusAuctionDelay` - 0
* `surplusBuffer` - 500,000 \(RAD\)

### SafeEngine

* `globalDebtCeiling` - MAX\_UINT
* `safeDebtCeiling` - 10B \(WAD\)
* `ETH-A debtFloor` - 800 \(RAD\)

### TaxCollector

* `ETH-A annual stability fee` - 2%
* `globalStabilityFee` - 0
* `maxSecondaryReceivers` - 5

### LiquidationEngine

* `ETH-A liquidationPenalty` - 12%
* `ETH-A liquidationQuantity` - 90,000 \(RAD\)
* `onAuctionSystemCoinLimit` - 10,000,000,000,000 \(10T\)

### CoinJoin

* `decimals` - 18

### SurplusAuctionHouse

* `bidDuration` - 3600
* `bidIncrease` - 3%
* `totalAuctionLength` - 259,200 \(3 days\)

### DebtAuctionHouse

* `amountSoldIncrease` - 20%
* `bidDecrease` - 3%
* `bidDuration` - 1800
* `totalAuctionLength` - 259,200 \(3 days\)

### DSPause

* `EXEC_TIME` - 259,200 \(3 days\)
* `MAX_DELAY` - 2,419,200 \(28 days\)
* `MAX_DELAY_MULTIPLIER` - 3
* `delay` - 14,400
* `delayMultiplier` - 3
* `maxScheduledTransactions` - 10
* `protestEnd` - 500
* `protesterLifetime` - 15

### Coin

* `version` - 1
* `symbol` - RAI
* `name` - Rai Reflex Index
* `decimals` - 18
* `chainId` - 1
* `PERMIT_TYPEHASH` - 0xea2aa0a1be11a07ed86d755c93467f4f82362b452371d1ba94d1715123511acb
* `DOMAIN_SEPARATOR` - 0x0cb557711fbe89adc93440596bbd9aa3e811730a4c4ca1a07cb5c0c50fd3eb1c

### OracleRelayer

* `ETH-A safetyCRatio` - 145%
* `ETH-A liquidationCRatio` - 145%
* `redemptionRateLowerBound` - 999987563275136890352859595
* `redemptionRateUpperBound` - 1000000267417929490714933462

### GlobalSettlement

* `shutdownCooldown` - 345,600 \(4 days\)

### StabilityFeeTreasury

* `expensesMultiplier` - 3
* `minimumFundsRequired` - 300,000 \(RAD\)
* `pullMinFundsThreshold` - 120,000 \(RAD\)
* `surplusTransferDelay` - 1,209,600 \(14 days\)
* `treasuryCapacity` - 400,000 \(RAD\)

### RateSetter

* `baseUpdateCallerReward` - 0 \(WAD\)
* `maxUpdateCallerReward` - 10 \(WAD\)
* `perSecondCallerRewardIncrease` - 1000192559420674483977255848 \(100% per hour\)
* `updateRateDelay` - 14,400 \(4 hours\)

### FixedDiscountCollateralAuctionHouse - ETH-A

* `collateralType` - ETH-A
* `discount` - 10%
* `lowerCollateralMedianDeviation` - 20%
* `lowerSystemCoinMedianDeviation` - 1.5%
* `upperCollateralMedianDeviation` - 20%
* `upperSystemCoinMedianDeviation` - 1.5%
* `minSystemCoinMedianDeviation` - 4%
* `minimumBid` - 25 \(WAD\)
* `totalAuctionLength` - 281,474,976,710,655

### RAI Medianizer

* `baseUpdateCallerReward` - 0 \(WAD\)
* `maxUpdateCallerReward` - 10 \(WAD\)
* `converterFeedScalingFactor` - 1 \(WAD\)
* `defaultAmountIn` - 1 \(WAD\)
* `granularity` - 4
* `maxWindowSize` - 93,600 \(26 hours\)
* `periodSize` - 14,400 \(4 hours\)
* `symbol` - RAIUSD
* `windowSize` - 57,600 \(16 hours\)

### ETH Medianizer

* `multiplier` - 10
* `staleThreshold` - 6 \(hours\)
* `symbol` - ETHUSD

