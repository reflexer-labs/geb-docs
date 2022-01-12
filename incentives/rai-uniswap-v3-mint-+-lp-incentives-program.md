# RAI Uniswap V3 Mint + LP Incentives Program

## Overview

The RAI Mint + LP strategy requires that participants mint RAI **and** provide RAI/DAI liquidity on a certain range on Uniswap v3. The rewards will be claimable monthly around the 15th of the month.\
\
The RAI/DAI 0.05% fee pool concerned is [here](https://info.uniswap.org/#/pools/0xcb0c5d9d92f4f2f80cce7aa271a1e148c226e19d).

There are 2 important conditions:

* The position needs to be at least 5 ticks wide (5 increments on the official Uniswap v3 UI, which is 0.5% wide) **and** include the market price and the redemption price. If the position is narrower than 5 ticks, or doesn't include the market price or the redemption price, you will not accrue any rewards. If your position is larger than 5 ticks you will earn less rewards depending on your liquidity concentration.&#x20;
* The RAI debt value in USD in the Safe, needs the cover the full LP position value (RAI + DAI) to earn maximum rewards. If the RAI debt is less than the total LP position value, a proportional discount is applied to your reward amount.&#x20;

## How It Works

1. Go to [app.reflexer.finance](https://app.reflexer.finance) or [DeFi Saver](https://app.defisaver.com/reflexer/manage) and mint some RAI.
2. Go to the [RAI/DAI Uniswap v3 pool](https://info.uniswap.org/#/pools/0xcb0c5d9d92f4f2f80cce7aa271a1e148c226e19d) and add the RAI you minted as liquidity

When choosing the range, make sure to include both the redemption and the market price:

![](../.gitbook/assets/Selection\_1126.png)

You can find the redemption price at [https://stats.reflexer.finance/](https://stats.reflexer.finance). The RAI market price will be displayed directly on the Uniswap v3 UI.

To help you to find the best range, we provide recommendations at [https://app.reflexer.finance/#/earn/incentives](https://app.reflexer.finance/#/earn/incentives).&#x20;

**Important:** both the redemption price and market price move over time. If any of the prices move outside of your LP range, you won't accrue any LP rewards. If you intend to move/rebalance your position less frequently, you can pick a wider range.&#x20;

#### Additional notes:

* We always assume 1 DAI = 1 USD for the calculations
* Rewards are calculated using [this](https://github.com/reflexer-labs/uni-v3-incentive-reward-script) script
* You **must** use the same address to mint RAI and provide Uniswap v3 RAI/ETH or RAI/DAI liquidity
* The Uniswap v3 RAI/DAI LP tokens **must** stay on the same address that you used to mint RAI and provide liquidity
* If you open multiple Safes with the same address, your total RAI debt will be the sum of all RAI minted by each of your Safes
* If you have several LP positions with the same address, the total sum of the liquidity will be considered for rewards.
* Only positions from the official Uniswap V3 NFT manager are supported (used by the official UI at [https://app.uniswap.org/#/pool](https://app.uniswap.org/#/pool)). Positions directly minted on the pool contract are **not** supported.
* If your Safe gets liquidated, the amount of minted RAI you have decreases by the amount of RAI that got confiscated

## Examples

Current redemption price: $3.0322\
Current market price: $3.03897 (DAI per RAI but we assume 1 DAI = 1 USD)

Bob: Mint 1000 RAI + LP between 3.0312 and 3.0403 for a total position value of $3038.97\
Alice: Mint 1000 RAI + LP between 3.0281 and 3.0433 for a total position value of $3038.97 \
Sophie: Mint 1000 RAI + LP between 3.0221 and 3.0524 for a total position value of $3038.97&#x20;

George: Mint 500 RAI + LP between 3.0281 and 3.0433 for a total position value of $3038.97\
Robert: Mint 1000 RAI + LP between 3.0281 and 3.0372 for a total position value of $3038.97\
Richard: Mint 1000 RAI + LP between 3.0342 and 3.0433 for a total position value of $3038.97

* Bob earns earns 0 FLX rewards because his range is narrower than 5 ticks
* Both Alice and Sophie are earning rewards. Alice earns twice more reward than Sophie because her range is half the size.&#x20;
* George earns about half the rewards that Alice gets because his debt value is only 500 RAI (= $1519.48) which is only half of his position value ($3038.97).
* Robert earns 0 FLX rewards because his range does not include the market price&#x20;
* Richard earns 0 FLX rewards because his range does not include the redemption price
