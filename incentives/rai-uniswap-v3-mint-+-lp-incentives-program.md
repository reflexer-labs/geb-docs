# RAI Uniswap V3 Mint + LP Incentives Program

## Overview

The RAI Mint + LP strategy requires that participants mint RAI **and** provide RAI/DAI liquidity on Uniswap v3 at the same time in order to accrue retroactive rewards.  
  
The RAI/DAI one is [here](https://info.uniswap.org/#/pools/0xcb0c5d9d92f4f2f80cce7aa271a1e148c226e19d).

**NOTE**: from 6th of October 2021 onward, Uniswap v3 RAI LPs should deploy liquidity in such a way as to include **both the RAI redemption price and the current market price** in their ranges.

## How It Works

1. Go to [app.reflexer.finance](https://app.reflexer.finance/) or [DeFi Saver](https://app.defisaver.com/reflexer/manage) and mint some RAI.
2. Go to the [RAI/DAI Uniswap v3 pool](https://info.uniswap.org/#/pools/0xcb0c5d9d92f4f2f80cce7aa271a1e148c226e19d) and add the RAI you minted as liquidity

You do **not** accrue rewards if:

* You provide RAI/DAI liquidity without minting RAI \(e.g buy from the pool and LP\)
* You mint RAI without adding RAI/DAI liquidity
* The RAI **market price** is outside or shifted off your liquidity band, even if the redemption price is still in your range

You accrue **less** rewards if:

* If the RAI/DAI market price shifts, resulting in the position owning less underlying RAI compared to your debt
* Your debt goes down due to repaying your debt or liquidation

The exact relevant metric used for rewards is: 

$$
min(\frac{\texttt{Sum of all safe debt}}{\texttt{Sum of all active RAI in LP}}, 1)\times \texttt{Sum of active UniV3 virtual liquidity}
$$

**Example:** 

ETH price: $3000, RAI Market Price: $3.03, RAI Redemption Price: $3.05

First, deposit 3 ETH to mint 1000 RAI \(Collateralization ratio around 300% and liquidation price around $1500\). Then deposit the 1000 RAI along with 3000 DAI into Uniswap V3 RAI/DAI. Let's look into 2 options for LP price range:

1. From $2.98 to $3.08 DAI = $0.10 wide range 
2. From $3.015 to $3.025 DAI = $0.01 wide range

Since the second option is about 10 times more concentrated, it will return 10 times more FLX rewards. However, if the price move above $3.025 or below $3.015 the second option will not give any rewards.

## LP Strategy

The rewards will be distributed considering how much each LP concentrates liquidity around RAI's redemption price and assuming the current RAI market price is in the range. To visualize this, let's take the example of Alice \(red\), Bob \(purple\) and Charlie \(green\) who LP in the RAI/ETH pool.

![Alice accrues the most rewards because her tight range includes both market and redemption prices](../.gitbook/assets/bob.png)

You can see that:

* Alice accrues the most rewards because she concentrated liquidity really close to the current RAI market price and her range also includes the redemption price
* Bob is not accruing any rewards because none of his liquidity includes the tick where the current 

  market price or redemption price is

* Charlie is accruing some rewards but less than Alice because he has a wider liquidity distribution around the market + redemption prices

To maximize rewards, LPs have to concentrate liquidity closer to the redemption price and also make sure the current market price is in their range \(**at the risk of higher impermanent loss**\).

## Important Notes

* You **must** use the same address to mint RAI and provide Uniswap v3 RAI/ETH or RAI/DAI liquidity
* The Uniswap v3 RAI/DAI LP tokens **must** stay on the same address that you used to mint RAI and provide liquidity
* If you open multiple Safes with the same address, your total RAI debt will be the sum of all RAI minted by each of your Safes
* If you have several LP positions with the same address, the total sum of the liquidity will be considered for rewards.
* Only positions from the official Uniswap V3 NFT manager are supported \(used by the official UI at [https://app.uniswap.org/\#/pool](https://app.uniswap.org/#/pool)\). Positions directly minted on the pool contract are **not** supported.
* If your Safe gets liquidated, the amount of minted RAI you have decreases by the amount of RAI that got confiscated

