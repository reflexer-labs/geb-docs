# Current System Parameters

## Mainnet

### Quick Glance

* Minimum amount of debt per Safe: 500
* Maximum amount of debt per Safe: 10B
* ETH-A annual borrow rate/stability fee: 2%
* Global debt ceiling: MAX\_UINT
* Liquidation penalty: 12%
* ETH-A auction fixed discount: 6% 
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

* `globalDebtCeiling` - 10,200 \(RAD\)
* `safeDebtCeiling` - 765 \(WAD\)
* `ETH-A debtCeiling` - 10,200 \(RAD\)
* `ETH-A debtFloor` - 85 \(RAD\)

### TaxCollector

* `ETH-A annual stability fee` - 2%
* `globalStabilityFee` - 0
* `maxSecondaryReceivers` - 5

### LiquidationEngine

* `ETH-A liquidationPenalty` - 12%
* `ETH-A liquidationQuantity` - 80,000 \(RAD\)
* `onAuctionSystemCoinLimit` - 850

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
* `delay` - 0
* `delayMultiplier` - 3
* `maxScheduledTransactions` - 10
* `protestEnd` - 500
* `protesterLifetime` - 15

### Coin

* `version` - 1
* `symbol` - RAI
* `name` - Rai Reflex Index
* `decimals` - 18
* `changeData` - 1
* `chainId` - 1
* `PERMIT_TYPEHASH` - 
* `DOMAIN_SEPARATOR` - 

### OracleRelayer

* `ETH-A orcl` - [0x5b89fF2DeCd360Aa01cbd453AA2cEd4F23b674b6](https://etherscan.io/address/0x5b89fF2DeCd360Aa01cbd453AA2cEd4F23b674b6)
* `ETH-A safetyCRatio` - 150%
* `ETH-A liquidationCRatio` - 150%
* `redemptionRateLowerBound` - 999997417323343052486607343
* `redemptionRateUpperBound` - 1000002110205430114080521087
* `safeEngine` - [0xf0b7808b940b78be81ad6f9e075ce8be4a837e2c](https://etherscan.io/address/0xf0b7808b940b78be81ad6f9e075ce8be4a837e2c)

### GlobalSettlement

* `accountingEngine` - [0x89e8bd799ab06dd7ee2be1325fafef1ab48676bc](https://etherscan.io/address/0x89e8bd799ab06dd7ee2be1325fafef1ab48676bc)
* `coinSavingsAccount` - [0x0000000000000000000000000000000000000000](https://etherscan.io/address/0x0000000000000000000000000000000000000000)
* `liquidationEngine` - [0x8ea70611850d13856877d9ed8035d07e80eb0b73](https://etherscan.io/address/0x8ea70611850d13856877d9ed8035d07e80eb0b73)
* `oracleRelayer` - [0x2b56976b6e95304f9b3d9736aaa610e963422ccd](https://etherscan.io/address/0x2b56976b6e95304f9b3d9736aaa610e963422ccd)
* `safeEngine` - [0xf0b7808b940b78be81ad6f9e075ce8be4a837e2c](https://etherscan.io/address/0xf0b7808b940b78be81ad6f9e075ce8be4a837e2c)
* `stabilityFeeTreasury` - [0xff6d7479c0882daa3212785adaf7786d1df09cb8](https://etherscan.io/address/0xff6d7479c0882daa3212785adaf7786d1df09cb8)
* `shutdownCooldown` - 10800 \(3 hours\)

### StabilityFeeTreasury

* `accountingEngine` - [0x89e8bd799ab06dd7ee2be1325fafef1ab48676bc](https://etherscan.io/address/0x89e8bd799ab06dd7ee2be1325fafef1ab48676bc)
* `coinJoin` - [0x138c3d13b633b5a5cb5db5faf27429eeed78b338](https://etherscan.io/address/0x138c3d13b633b5a5cb5db5faf27429eeed78b338)
* `safeEngine` - [0xf0b7808b940b78be81ad6f9e075ce8be4a837e2c](https://etherscan.io/address/0xf0b7808b940b78be81ad6f9e075ce8be4a837e2c)
* `systemCoin` - [0x715c3830fb0c4bab9a8e31c922626e1757716f3a](https://etherscan.io/address/0x715c3830fb0c4bab9a8e31c922626e1757716f3a)
* `expensesMultiplier` - 2
* `minimumFundsRequired` - 50 \(RAD\)
* `surplusTransferDelay` - 1,814,400 \(21 days\)
* `treasuryCapacity` - 200,000 \(RAD\)

### RateSetter

* `oracleRelayer` - [0x2b56976b6e95304f9b3d9736aaa610e963422ccd](https://etherscan.io/address/0x2b56976b6e95304f9b3d9736aaa610e963422ccd)
* `orcl` - [0x206d268c0bbf3fbd8dab35ba91ca89203a3c59aa](https://etherscan.io/address/0x206d268c0bbf3fbd8dab35ba91ca89203a3c59aa)
* `treasury` - [0xff6d7479c0882daa3212785adaf7786d1df09cb8](https://etherscan.io/address/0xff6d7479c0882daa3212785adaf7786d1df09cb8)
* `baseUpdateCallerReward` - 0.5 \(WAD\)
* `maxUpdateCallerReward` - 1 \(WAD\)
* `perSecondCallerRewardIncrease` - 1000192559420674483977255848 \(100% per hour\)
* `pidValidator` - [0x64ad684378770fe3ee4b437737edf379f12a902a](https://etherscan.io/address/0x64ad684378770fe3ee4b437737edf379f12a902a)
* `updateRateDelay` - 3600

### FixedDiscountCollateralAuctionHouse - ETH-A

* `collateralFSM` - [0x5b89ff2decd360aa01cbd453aa2ced4f23b674b6](https://etherscan.io/address/0x5b89ff2decd360aa01cbd453aa2ced4f23b674b6)
* `collateralMedian` - [0x9f816fce2885f4dc65a7342b57ced29655fca712](https://etherscan.io/address/0x9f816fce2885f4dc65a7342b57ced29655fca712)
* `liquidationEngine` - [0x8ea70611850d13856877d9ed8035d07e80eb0b73](https://etherscan.io/address/0x8ea70611850d13856877d9ed8035d07e80eb0b73)
* `oracleRelayer` - [0x2b56976b6e95304f9b3d9736aaa610e963422ccd](https://etherscan.io/address/0x2b56976b6e95304f9b3d9736aaa610e963422ccd)
* `safeEngine` - [0xf0b7808b940b78be81ad6f9e075ce8be4a837e2c](https://etherscan.io/address/0xf0b7808b940b78be81ad6f9e075ce8be4a837e2c)
* `systemCoinOracle` - [0x0000000000000000000000000000000000000000](https://etherscan.io/address/0x0000000000000000000000000000000000000000)
* `collateralType` - ETH-A
* `discount` - 5%
* `lowerCollateralMedianDeviation` - 15%
* `lowerSystemCoinMedianDeviation` - 0%
* `upperCollateralMedianDeviation` - 15%
* `upperSystemCoinMedianDeviation` - 0%
* `minSystemCoinMedianDeviation` - 0.1%
* `minimumBid` - 5 \(WAD\)
* `totalAuctionLength` - 281,474,976,710,655 \(approx 8,925,512 years\)

### PRAI Medianizer

* `treasury` - [0xff6d7479c0882daa3212785adaf7786d1df09cb8](https://etherscan.io/address/0xff6d7479c0882daa3212785adaf7786d1df09cb8)
* `converterFeed` - [0x9f816fce2885f4dc65a7342b57ced29655fca712](https://etherscan.io/address/0x9f816fce2885f4dc65a7342b57ced29655fca712)
* `denominationToken` - [0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2](https://etherscan.io/address/0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2)
* `targetToken` - [0x715c3830fb0c4bab9a8e31c922626e1757716f3a](https://etherscan.io/address/0x715c3830fb0c4bab9a8e31c922626e1757716f3a)
* `uniswapFactory` - [0x5c69bee701ef814a2b6a3edd4b1652cb9cc5aa6f](https://etherscan.io/address/0x5c69bee701ef814a2b6a3edd4b1652cb9cc5aa6f)
* `uniswapPair` - [0xebde9f61e34b7ac5aae5a4170e964ea85988008c](https://etherscan.io/address/0xebde9f61e34b7ac5aae5a4170e964ea85988008c)
* `baseUpdateCallerReward` - 0.5 \(WAD\)
* `maxUpdateCallerReward` - 1 \(WAD\)
* `converterFeedScalingFactor` - 1 \(WAD\)
* `defaultAmountIn` - 1 \(WAD\)
* `granularity` - 12
* `maxRewardIncreaseDelay` - 10,800 \(6 hours\)
* `maxWindowSize` - 57,600 \(16 hours\)
* `perSecondCallerRewardIncrease` - 1000192559420674483977255848 \(100% per hour\)
* `periodSize` - 3600
* `symbol` - PRAIUSD
* `windowSize` - 43,200 \(12 hours\)

### PRAI FSM

* `priceSource` - [0xca8a1a8e6d83c2b2e933e05a5099cf814169e7d4](https://etherscan.io/address/0xca8a1a8e6d83c2b2e933e05a5099cf814169e7d4)
* `newPriceDeviation` - 20%

### ETH Medianizer

* `chainlinkAggregator` - [0x5f4ec3df9cbd43714fe2740f5e3616155c5b8419](https://etherscan.io/address/0x5f4ec3df9cbd43714fe2740f5e3616155c5b8419)
* `treasury` - [0xff6d7479c0882daa3212785adaf7786d1df09cb8](https://etherscan.io/address/0xff6d7479c0882daa3212785adaf7786d1df09cb8)
* `baseUpdateCallerReward` - 0.5 \(WAD\)
* `maxUpdateCallerReward` - 1 \(WAD\)
* `maxRewardIncreaseDelay` - 10,800 \(6 hours\)
* `multiplier` - 10
* `perSecondCallerRewardIncrease` - 1000192559420674483977255848 \(100% per hour\)
* `periodSize` - 3600
* `staleThreshold` - 6 \(hours\)
* `symbol` - ETHUSD

