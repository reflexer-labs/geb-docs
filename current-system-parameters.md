# Current System Parameters

## Mainnet

### Quick Glance

* Minimum amount of debt per Safe: 85
* Maximum amount of debt per Safe: 765
* ETH-A annual borrow rate/stability fee: 1.5%
* Global debt ceiling: 10,200
* Liquidation penalty: 11%
* ETH-A auction fixed discount: 5% 
* ETH-A fixed discount auction minimum bid: 5 \(WAD\)

### AccountingEngine

* `debtAuctionBidSize` - 50,000 \(RAD\)
* `debtAuctionHouse` - [0x49c3dd1d66d2919611dbde40de088e85b9f96851](https://etherscan.io/address/0x49c3dd1d66d2919611dbde40de088e85b9f96851)
* `disableCooldown` - 1814400 \(21 days\)
* `initialDebtAuctionMintedTokens` - 252 \(WAD\)
* `popDebtDelay` - 1036800 \(12 days\)
* `postSettlementSurplusDrain` - [0x0000000000000000000000000000000000000000](https://etherscan.io/address/0x0000000000000000000000000000000000000000)
* `protocolTokenAuthority` - [0x05d60fee5e7169b64a66487ab76123c31371d38c](https://etherscan.io/address/0x05d60fee5e7169b64a66487ab76123c31371d38c)
* `safeEngine` - [0xf0b7808b940b78be81ad6f9e075ce8be4a837e2c](https://etherscan.io/address/0xf0b7808b940b78be81ad6f9e075ce8be4a837e2c)
* `surplusAuctionAmountToSell` - 6,000 \(RAD\)
* `surplusAuctionDelay` - 0
* `surplusAuctionHouse` - [0xe8bd8179da781d17383708d0831b1da1efa85a57](https://etherscan.io/address/0xe8bd8179da781d17383708d0831b1da1efa85a57)
* `surplusBuffer` - 300,000 \(RAD\)

### SafeEngine

* `globalDebtCeiling` - 10,200 \(RAD\)
* `safeDebtCeiling` - 765 \(WAD\)
* `ETH-A debtCeiling` - 10,200 \(RAD\)
* `ETH-A debtFloor` - 85 \(RAD\)

### TaxCollector

* `ETH-A per-second stability fee` - 1000000000472114805215157978
* `ETH-A annual stability fee` - 1.5%
* `globalStabilityFee` - 0
* `maxSecondaryReceivers` - 3
* `primaryTaxReceiver` - [0x89e8bd799ab06dd7ee2be1325fafef1ab48676bc](https://etherscan.io/address/0x89e8bd799ab06dd7ee2be1325fafef1ab48676bc)
* `safeEngine` - [0xf0b7808b940b78be81ad6f9e075ce8be4a837e2c](https://etherscan.io/address/0xf0b7808b940b78be81ad6f9e075ce8be4a837e2c)
* `secondaryReceiverAccounts` - \[[0xfF6D7479C0882dAa3212785adAF7786d1Df09cB8](https://etherscan.io/address/0xfF6D7479C0882dAa3212785adAF7786d1Df09cB8)\]
* `secondaryReceiverAllotedTax` - \[\[50% `ETH-A`\]\]

### LiquidationEngine

* `accountingEngine` - [0x89e8bd799ab06dd7ee2be1325fafef1ab48676bc](https://etherscan.io/address/0x89e8bd799ab06dd7ee2be1325fafef1ab48676bc)
* `ETH-A liquidationPenalty` - 11%
* `ETH-A liquidationQuantity` - 50,000 \(RAD\)
* `ETH-A collateralAuctionHouse` - [0x191ba53C077D3E9eC2CBE6f52Eb0a33Ea1A226f2](https://etherscan.io/address/0x191ba53C077D3E9eC2CBE6f52Eb0a33Ea1A226f2)
* `onAuctionSystemCoinLimit` - 850
* `safeEngine` - [0xf0b7808b940b78be81ad6f9e075ce8be4a837e2c](https://etherscan.io/address/0xf0b7808b940b78be81ad6f9e075ce8be4a837e2c)

### CoinJoin

* `decimals` - 18
* `safeEngine` - [0xf0b7808b940b78be81ad6f9e075ce8be4a837e2c](https://etherscan.io/address/0xf0b7808b940b78be81ad6f9e075ce8be4a837e2c)
* `systemCoin` - [0x715c3830fb0c4bab9a8e31c922626e1757716f3a](https://etherscan.io/address/0x715c3830fb0c4bab9a8e31c922626e1757716f3a)

### PreSettlementSurplusAuctionHouse

* `bidDuration` - 3600
* `bidIncrease` - 3%
* `protocolToken` - [0x0000000000000000000000000000000000000000](https://etherscan.io/address/0x0000000000000000000000000000000000000000)
* `safeEngine` - [0xf0b7808b940b78be81ad6f9e075ce8be4a837e2c](https://etherscan.io/address/0xf0b7808b940b78be81ad6f9e075ce8be4a837e2c)
* `totalAuctionLength` - 259200 \(3 days\)

### DebtAuctionHouse

* `accountingEngine` - [0x89e8bd799ab06dd7ee2be1325fafef1ab48676bc](https://etherscan.io/address/0x89e8bd799ab06dd7ee2be1325fafef1ab48676bc)
* `amountSoldIncrease` - 20%
* `bidDecrease` - 3%
* `bidDuration` - 1800
* `protocolToken` - [0x0000000000000000000000000000000000000000](https://etherscan.io/address/0x0000000000000000000000000000000000000000)
* `safeEngine` - [0xf0b7808b940b78be81ad6f9e075ce8be4a837e2c](https://etherscan.io/address/0xf0b7808b940b78be81ad6f9e075ce8be4a837e2c)
* `totalAuctionLength` - 259,200 \(3 days\)

### DSPause

* `EXEC_TIME` - 259,200 \(3 days\)
* `MAX_DELAY` - 2,419,200 \(28 days\)
* `MAX_DELAY_MULTIPLIER` - 3
* `authority` - [0x92321cf8530fe33e9b36750154922a55306d5143](https://etherscan.io/address/0x92321cf8530fe33e9b36750154922a55306d5143)
* `delay` - 0
* `delayMultiplier` - 3
* `deploymentTime` - 1603567488
* `maxScheduledTransactions` - 10
* `owner` - [0x0000000000000000000000000000000000000000](https://etherscan.io/address/0x0000000000000000000000000000000000000000)
* `protestEnd` - 500
* `protester` - [0x0000000000000000000000000000000000000000](https://etherscan.io/address/0x0000000000000000000000000000000000000000)
* `protesterLifetime` - 4,7304,000 \(547.5 days\)
* `proxy` - [0xd487eab6902295b650c8940277bd07f684ce91ad](https://etherscan.io/address/0xd487eab6902295b650c8940277bd07f684ce91ad)

### Coin

* `version` - 2
* `symbol` - PRAI
* `name` - Proto Rai Reflex Index
* `decimals` - 18
* `changeData` - 1
* `chainId` - 1
* `PERMIT_TYPEHASH` - 0xea2aa0a1be11a07ed86d755c93467f4f82362b452371d1ba94d1715123511acb
* `DOMAIN_SEPARATOR` - 0x76072887f0adddb5efcaa9dfb2a7a04e92f0ffb0ece22dfd0d43ee1deaea1f6e

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
* `shutdownCooldown` - 345,600 \(4 days\)

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



