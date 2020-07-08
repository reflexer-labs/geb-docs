---
description: >-
  Oracle component that tries to find the "true" price of an asset by
  medianizing multiple results
---

# Medianizer

There are three types of medianizers that can be used in GEB:

1. `DSValue` - used when deploying the system on a localchain or a testnet in order to easily manipulate the price feed of a collateral type / system coin.
2. `GovernanceLed/ChainlinkPriceFeedMedianizer` - offers more security by requiring a `quorum` of oracles to submit their prices which are then medianized on-chain.
3. `OracleNetworkMedianizer` - work in progress. First discussed on [ethresear.ch](https://ethresear.ch/t/metacoin-governance-minimized-oracle/7293) and later proposed in the Rai [whitepaper](https://github.com/reflexer-labs/whitepapers/blob/master/rai.pdf).



