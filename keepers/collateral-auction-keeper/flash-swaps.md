---
description: Lightning fast bidding in collateral auctions
---

# Collateral Auction Flash Swaps

## Overview

Normally, the collateral [auction-keeper](https://github.com/reflexer-labs/auction-keeper) uses system coins to bid in fixed discount collateral auctions. A keeper operator must acquire system coins to be prepared for future collateral auctions. With collateral auction flash swaps, this is no longer necessary.

The auction-keeper can be configured to use Uniswap v2 or v3 flash swaps when bidding in collateral auctions. This allows the keeper to participate in collateral auctions with no upfront capital other than gas costs to execute the transaction.

## Details

The collateral auction-keeper performs flash swaps using the [geb-keeper-flash-proxy](https://github.com/reflexer-labs/geb-keeper-flash-proxy) contracts.

The flash proxy is used by the auction-keeper to:

1\) Liquidate an underwater SAFE, start a new collateral auction and bid by calling `liquidateAndSettleSAFE`

or

2\) Bid in existing auctions by calling `settleAuction`

### Uniswap Pools Used for Bidding

* [RAI/DAI Uniswap V3](https://info.uniswap.org/#/pools/0xcb0c5d9d92f4f2f80cce7aa271a1e148c226e19d)
* [RAI/ETH Uniswap V2](https://v2.info.uniswap.org/pair/0x8ae720a71622e824f576b4a8c03031066548a3b1)
* [RAI/USDC Uniswap V3](https://info.uniswap.org/#/pools/0xfa7d7a0858a45c1b3b7238522a0c0d123900c118)
* [RAI/ETH Uniswap V3](https://info.uniswap.org/#/pools/0x14de8287adc90f0f95bf567c0707670de52e3813)

## Configuration

You can enable flashswaps using the `--flash-swaps` flag when starting a collateral auction-keeper.

You can also specify multiple pools that the keeper loops through when trying to bid in an auction. You do this using the `--flash-swap-pools` flag. For example: 

```text
--flash-swap-pools eth-v2, dai-v3, usdc-v3, eth-v3
```

If a flash swap for a specific pool fails, the next in order will be tried.

The default order is `dai-v3, eth-v2, usdc-v3, eth-v3`.

## Flash Swaps in Action

Below is log output from _auction-keeper_ started with `--flash-swap` and `--flash-swap-pools usdc-v3,dai-v3,eth-v2,eth-v3`

First, the _auction-keeper_ finds a critical SAFE and successfully calls `liquidateAndSettleSAFE` using the usdc-v3 flash proxy. The _auction-keeper_ finds another critical SAFE and tries to call `liquidateAndSettleSAFE` using the usdc-v3 flash proxy again. As this is a much larger SAFE, this call fails due to lack of liquidity in rai-usdc-v3 pool. The _auction-keeper_ then successfully uses the dai-v3 flash-proxy to liquidate this SAFE.

Finally, the _auction-keeper_ finds a third ciritcal SAFE and successfully uses usdc-v3 flash proxy to `liquidateAndSettleSAFE`

```text
2021-08-26 14:25:18,258 INFO     Keeper connected to RPC connection http://172.31.46.114:8545
2021-08-26 14:25:18,259 INFO     Keeper operating as 0xdD1693BD8E306eCfDbe41D246562fc4109f871f8
2021-08-26 14:25:18,289 INFO     Executing keeper startup logic
2021-08-26 14:25:19,375 INFO     Keeper will not settle auctions
2021-08-26 14:25:19,375 INFO     Keeper will perform the following operation(s) in parallel:
2021-08-26 14:25:19,375 INFO     --> Check all safes and start new auctions if any critical safes need to be liquidated
2021-08-26 14:25:19,375 INFO     --> Check all auctions being monitored and evaluate bidding opportunity every 4.0 seconds
2021-08-26 14:25:19,375 INFO     *** When Keeper is settling/bidding, the initial evaluation of auctions will likely take > 45 minutes without setting a lower boundary via '--min-auction' ***
2021-08-26 14:25:19,375 INFO     *** When Keeper is starting auctions, initializing safe history may take > 30 minutes without using Graph via `--graph-endpoints` ***
2021-08-26 14:25:19,378 INFO     Keeper will use Node gas price (currently 2.0 Gwei, changes over time) with initial multiplier 1.0 and will multiply by 1.125 every 42s to a maximum of 2000.0 Gwei for transactions and bids unless model instructs otherwise
2021-08-26 14:25:19,658 INFO     Watching for new blocks
2021-08-26 14:25:19,664 INFO     Started 2 timer(s)
2021-08-26 14:25:20,750 INFO     Getting safe mods from https://api.thegraph.com/subgraphs/name/reflexer-labs/rai-kovan
2021-08-26 14:25:20,752 INFO     Fetching safe modes from https://api.thegraph.com/subgraphs/name/reflexer-labs/rai-kovan
2021-08-26 14:25:42,418 INFO     Using usdc-v3 flash swap to liquidate and settle safe 0xA0be17B5C408aaFF03e0E3434de199F6df94fFC7
2021-08-26 14:25:42,946 INFO     Sent transaction GebETHKeeperFlashProxy('0x8a8970c74Ca60c954b32D1015D4563908eB798Fd').liquidateAndSettleSAFE('0xA0be17B5C408aaFF03e0E3434de199F6df94fFC7') with nonce=4964, gas=2000000, gas_price=2000000000 (tx_hash=0xdf19ae2f76def32a3c797ef51b614ddf6b680ad38ecbe3910b154a4c4228eb0f)
2021-08-26 14:25:44,423 INFO     Transaction GebETHKeeperFlashProxy('0x8a8970c74Ca60c954b32D1015D4563908eB798Fd').liquidateAndSettleSAFE('0xA0be17B5C408aaFF03e0E3434de199F6df94fFC7') was successful (tx_hash=0xdf19ae2f76def32a3c797ef51b614ddf6b680ad38ecbe3910b154a4c4228eb0f)
2021-08-26 14:25:44,519 INFO     Using usdc-v3 flash swap to liquidate and settle safe 0xa46B5d01326d858856396fb80bD06654ED64770f
2021-08-26 14:25:44,820 WARNING  Transaction GebETHKeeperFlashProxy('0x8a8970c74Ca60c954b32D1015D4563908eB798Fd').liquidateAndSettleSAFE('0xa46B5d01326d858856396fb80bD06654ED64770f') will fail, refusing to send (execution reverted: failed bidding)
2021-08-26 14:25:44,821 WARNING  flash swap liquidate and settle with pool usdc-v3 failed.
2021-08-26 14:25:44,821 INFO     Using dai-v3 flash swap to liquidate and settle safe 0xa46B5d01326d858856396fb80bD06654ED64770f
2021-08-26 14:25:45,223 INFO     Sent transaction GebETHKeeperFlashProxy('0x26232B7Fe0dD0Ade6192Bd69438dBEc424EF7a59').liquidateAndSettleSAFE('0xa46B5d01326d858856396fb80bD06654ED64770f') with nonce=4965, gas=2000000, gas_price=2000000000 (tx_hash=0xb4c177c114874ee267a8e2d47a3bd075936372996eeebbf3caf1decb881f2737)
2021-08-26 14:25:52,724 INFO     Transaction GebETHKeeperFlashProxy('0x26232B7Fe0dD0Ade6192Bd69438dBEc424EF7a59').liquidateAndSettleSAFE('0xa46B5d01326d858856396fb80bD06654ED64770f') was successful (tx_hash=0xb4c177c114874ee267a8e2d47a3bd075936372996eeebbf3caf1decb881f2737)
2021-08-26 14:25:53,171 INFO     Using usdc-v3 flash swap to liquidate and settle safe 0x20aE373C782b03925413a394EE620A4BF2828a60
2021-08-26 14:25:53,744 INFO     Sent transaction GebETHKeeperFlashProxy('0x8a8970c74Ca60c954b32D1015D4563908eB798Fd').liquidateAndSettleSAFE('0x20aE373C782b03925413a394EE620A4BF2828a60') with nonce=4966, gas=2000000, gas_price=2000000000 (tx_hash=0xf4d45385753219c5a9a7fb28f7d60e0e4b4d42eb05d8d65f50deb09308cfd1ba)
2021-08-26 14:25:56,851 INFO     Transaction GebETHKeeperFlashProxy('0x8a8970c74Ca60c954b32D1015D4563908eB798Fd').liquidateAndSettleSAFE('0x20aE373C782b03925413a394EE620A4BF2828a60') was successful (tx_hash=0xf4d45385753219c5a9a7fb28f7d60e0e4b4d42eb05d8d65f50deb09308cfd1ba)
```

## Caveats

* Flash swaps are only supported for collateral auctions.
* _auction-keeper_ will not do flash swaps on critical SAFEs with saviours. [Read more about saviours](https://docs.reflexer.finance/integrations/safe-insurance)

## Possible errors

When liquidity is too low, the calls to the flash proxys will revert _or_ run out of gas. In this case, the _auction-keeper_ will attempt the next uniswap pool in order. In the unlikely case all call fails\(none of the pools have enough liquidity\), the auction-keeper can be restarted without `--flash-swap` to bid normally with the keeper’s system coin.

