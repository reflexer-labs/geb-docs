---
description: Lightning fast bidding in collateral auctions
---

# Collateral Auction Flash Swaps

## Overview

Normally, the collateral [auction-keeper](https://github.com/reflexer-labs/auction-keeper) uses system coins to bid in fixed discount collateral auctions. A keeper operator must acquire system coins to be prepared for future collateral auctions. With collateral auction flash swaps, this is no longer necessary.

The auction-keeper can be configured to use [Uniswap v2 flash swaps](https://uniswap.org/docs/v2/core-concepts/flash-swaps/) when bidding in collateral auctions. This allows the keeper to participate in collateral auctions with no upfront capital other than gas costs to execute the transaction.

## Details

The collateral auction-keeper performs flash swaps using the [geb-keeper-flash-proxy](https://github.com/reflexer-labs/geb-keeper-flash-proxy) contracts.

The flash proxy is used by the auction-keeper to:

1\) Liquidate an underwater SAFE, start a new collateral auction and bid by calling `liquidateAndSettleSAFE`

or

2\) Bid in existing auctions by calling `settleAuction`

See more details for these functions in the [geb-keeper-flash-proxy source code](https://github.com/reflexer-labs/geb-keeper-flash-proxy/blob/master/src/GebUniswapV2KeeperFlashProxyETH.sol).

## Configuration

Use the `--flash-swaps` flag when starting a collateral auction-keeper.

## Flash Swaps in Action

Below you can find the log output from an auction keeper started with the `--flash-swap` flag.

First, the auction keeper finds a critical SAFE and calls `liquidateAndSettleSAFE` in the flash proxy. The keeper then detects two existing auctions and calls `settleAuction` for each one.

```text
[ec2-user@ip-172-31-40-135 ~]$ ./run_collateral_auction_keeper.sh 
upstream: Pulling from reflexer/auction-keeper
Digest: sha256:d222e4d8d948262af15fbc09e64973a6df65eb0f96023ea3ec7179674d87f28c
Status: Image is up to date for reflexer/auction-keeper:upstream
docker.io/reflexer/auction-keeper:upstream
Password for /keystore/keystore.json: 
2021-01-20 05:32:56,119 INFO     Keeper connected to RPC connection https://myparitynode.com
2021-01-20 05:32:56,120 INFO     Keeper operating as 0xaD1693BD8E307eCfDbe51D246562fc4109f871f8
2021-01-20 05:32:56,140 INFO     Executing keeper startup logic
2021-01-20 05:32:57,205 INFO     Keeper will not settle auctions
2021-01-20 05:32:57,206 INFO     Keeper will perform the following operation(s) in parallel:
2021-01-20 05:32:57,206 INFO     --> Check all safes and start new auctions if any critical safes need to be liquidated
2021-01-20 05:32:57,207 INFO     --> Check all auctions being monitored and evaluate bidding opportunity every 4.0 seconds
2021-01-20 05:32:57,207 INFO     *** When Keeper is settling/bidding, the initial evaluation of auctions will likely take > 45 minutes without setting a lower boundary via '--min-auction' ***
2021-01-20 05:32:57,207 INFO     *** When Keeper is starting auctions, initializing safe history may take > 30 minutes without using Graph via `--graph-endpoints` ***
2021-01-20 05:32:57,211 INFO     Keeper will use Node gas price (currently 10.0 Gwei, changes over time) with initial multiplier 1.0 and will multiply by 1.125 every 42s to a maximum of 2000.0 Gwei for transactions and bids unless model instructs otherwise
2021-01-20 05:32:57,642 INFO     Watching for new blocks
2021-01-20 05:32:57,644 INFO     Started 2 timer(s)
2021-01-20 05:33:04,715 INFO     Using flash swap to liquidate and settle safe SAFE('0xDa0b9d7e17d3cab1fBC3cBA7E3D23a88A61f73b1')[[ETH-A] locked_collateral=2.000000000000000000 generated_debt=449.717453246272262264]
2021-01-20 05:33:05,666 INFO     Sent transaction GebETHKeeperFlashProxy('0x4E452762915834B83e05533678a4D2F3F10cbBd2').liquidateAndSettleSAFE('0xDa0b9d7e17d3cab1fBC3cBA7E3D23a88A61f73b1') with nonce=1550, gas=1000000, gas_price=10000000000 (tx_hash=0x370092fa3a7aa78a611f93fa5d6d44f3ce17f8b06f280c74162f139e53fd2ce7)
2021-01-20 05:33:05,970 INFO     Checked 32 safes in 3 seconds
2021-01-20 05:33:06,213 INFO     Using flash swap to settle auction 14
2021-01-20 05:33:06,797 INFO     Sent transaction GebETHKeeperFlashProxy('0x4E452762915834B83e05533678a4D2F3F10cbBd2').settleAuction(uint256)(14) with nonce=1551, gas=1000000, gas_price=default (tx_hash=0x13dac6cf75de107890392e227a24201c24a3796fd930913f0ce933149889d3ff)
2021-01-20 05:33:13,218 INFO     Transaction GebETHKeeperFlashProxy('0x4E452762915834B83e05533678a4D2F3F10cbBd2').settleAuction(uint256)(14) was successful (tx_hash=0x13dac6cf75de107890392e227a24201c24a3796fd930913f0ce933149889d3ff)
2021-01-20 05:33:13,233 INFO     Using flash swap to settle auction 15
2021-01-20 05:33:13,290 INFO     Transaction GebETHKeeperFlashProxy('0x4E452762915834B83e05533678a4D2F3F10cbBd2').liquidateAndSettleSAFE('0xDa0b9d7e17d3cab1fBC3cBA7E3D23a88A61f73b1') was successful (tx_hash=0x370092fa3a7aa78a611f93fa5d6d44f3ce17f8b06f280c74162f139e53fd2ce7)
2021-01-20 05:33:14,238 INFO     Sent transaction GebETHKeeperFlashProxy('0x4E452762915834B83e05533678a4D2F3F10cbBd2').settleAuction(uint256)(15) with nonce=1552, gas=1000000, gas_price=default (tx_hash=0x0a89110fd8f11c7d52bf249848811c9f3ec5cc98b46bdee36ff8137bd9c99ba6)
2021-01-20 05:33:21,180 INFO     Transaction GebETHKeeperFlashProxy('0x4E452762915834B83e05533678a4D2F3F10cbBd2').settleAuction(uint256)(15) was successful (tx_hash=0x0a89110fd8f11c7d52bf249848811c9f3ec5cc98b46bdee36ff8137bd9c99ba6)
2021-01-20 05:33:21,187 INFO     Checked auctions 0 to 16 in 15 seconds
2021-01-20 05:33:22,136 INFO     Checked 32 safes in 0 seconds
```

## Caveats

* Flash swaps are only supported for collateral auctions
* auction-keeper will not do flash swaps on critical SAFEs with attached saviours. Read more about saviours [here](https://docs.reflexer.finance/integrations/safe-insurance)

## Possible errors

`GebUniswapV2KeeperFlashProxyETH/profit-not-enough-to-repay-the-flashswap`

If this happens, it means that the current system market price is too high in relation to the redemption price and repaying the swap will not yield a profit. Also, the swap itself will move the system market price up. 

This error can occur if there is not enough liquidity in the Uniswap pool to support the swap. In either case, the auction-keeper can be restarted without `--flash-swap` to bid normally using the keeper’s system coin balance.

