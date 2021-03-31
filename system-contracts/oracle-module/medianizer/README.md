---
description: Oracle component that fetches the price of an asset
---

# Medianizer

There are five types of medianizers that can be used in GEB:

1. `DSValue` - used when deploying the system on a localchain or a testnet in order to easily manipulate the price feed of a collateral type / system coin.
2. `GovernanceLedPriceFeedMedianizer`- offers more security by requiring a `quorum` of oracles to submit their prices which are then medianized on-chain.
3. `ChainlinkPriceFeedMedianizer` - relies on a Chainlink aggregator in order to fetch and push price feeds in to the system.
4. `UniswapConsecutiveSlotsPriceFeedMedianizer` - computes the average price for an asset in the last `m` seconds and can be connected to a `GovernanceLedPriceFeedMedianizer`or `ChainlinkPriceFeed` medianizer in order to translate the asset's price \(e.g use the WETH/WBTC pool to get the price of WBTC in terms of WETH\) in a different currency \(e.g translate the WBTC/WETH Uniswap pool price into USD\).



