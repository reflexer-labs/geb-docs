# Uni-V2 RAI/ETH Savior Math

The Uniswap V2 RAI/ETH savior allows users to deposit LP shares to protect their safe. Upon a liquidation attempt from a keeper, the savior will withdraw all RAI and ETH liquidity from Uniswap.&#x20;

The saviour will then repay as much RAI debt as possible. If the Safe's collateralization ratio is still not at the target ratio that the Safe owner picked, the saviour will also add more ETH in the Safe. The keeper that triggered the saviour will then take a flat fee as reward for saving the Safe.&#x20;

The savior does **not** guarantee that the safe will be saved. The user needs to make sure that they deposited enough LP tokens so the saviour can both reward the keeper that saves their position and bring the Safe's collateralization ratio to the target they picked.

### Minimum Savior Balance Formula

In order to derive the formula for the minimum LP balance that can bring your Safe to a target CRatio, we use the following variables:

**Liquidation price**: $$P_{liq}$$

**Redemption price**: $$P_{RP}$$

**Asset price**: $$P_{RAI}$$ , $$P_{ETH}$$

**Debt**: $$D$$

**Collateral**: $$C$$

**Accumulated rate**: $$A_{cc}$$

**CRatio**: $$R_{liq}$$ , $$R_{targ}$$

**Uniswap reserves**: $$S_{RAI}$$ , $$S_{ETH}$$

We first need to use the liquidation price formula:

$$
P_{liq} = \frac{DA_{cc}R_{liq}P_{rp}}{C}
$$

​From here, we can reorganize the formula to isolate the debt and collateral amount in the Safe. We also use the Target Rebalance CRatio $$R_{tar}$$ instead of the liquidation CRatio to know how much debt and collateral we need to save the Safe:

$$
\frac{C_{sav}}{D_{save}}  = \frac{A_{cc}R_{tar}P_{rp}}{P_{liq}}
$$

The savior will be withdrawing an amount of LP tokens to get $$D_{lp}$$ RAI and $$C_{LP}$$ ETH. Therefore, the resulting debt and collateral in safe after and adding the proceeds from the LP shares will be:&#x20;

$$C_{save} = C + C_{lp}$$&#x20;

$$D_{save} = C - D_{lp}$$

Which then gives us:

$$
\frac{C + C_{lp}}{D - D_{lp}}  = \frac{A_{cc}R_{tar}P_{rp}}{P_{liq}}
$$

We will reuse this formula later. We now need to model the Uniswap pool dynamics to calculate the amount $$D_{lp}$$ and $$C_{LP}$$  we would get from the savior LP shares at a given liquidation price. We start from the standard xy=k assumptions:

$$
\begin{cases}K_{lp} = S_{ETH}S_{RAI}\\ S_{ETH}P_{ETH}= S_{RAI}P_{RAI}\end{cases}
$$

$$
\iff K_{LP} P_{ETH} = {S_{RAI}}^2 P_{RAI}
$$

$$
\iff S_{RAI} = \sqrt{K_{LP} \frac{P_{ETH}}{P_{RAI}}}
$$

And similarly:

$$
\iff S_{ETH} = \sqrt{K_{LP} \frac{P_{RAI}}{P_{ETH}}}
$$

We can now replace these results in the equation from the liquidation price:

$$
\frac{C + \sqrt{K_{LP} \frac{P_{RAI}}{P_{ETH}}} }{D - \sqrt{K_{LP} \frac{P_{ETH}}{P_{RAI}}} }  = \frac{A_{cc}R_{tar}P_{rp}}{P_{liq}}
$$

We simplify the equation using $$p = \frac{P_{RAI}}{P_{ETH}}$$ and $$j=\frac{A_{cc}R_{tar}P_{rp}}{P_{liq}}$$

$$
\frac{C + \sqrt{K_{LP}p } }{D - \sqrt{K_{LP} \frac{1}{p}} }  = j
$$

We need to solve the equation above for $$K_{LP}$$.

Using Wolfram Alpha:

{% embed url="https://www.wolframalpha.com/input?i=solve%5C%2840%29Divide%5Bc%2BSqrt%5Bk*p%5D%2Cd-Sqrt%5Bk*%5C%2840%29Divide%5B1%2Cp%5D%5C%2841%29%5D%5D%3Dj%5C%2844%29k%5C%2841%29&i2d=true" %}

​We get 2 solutions:

$$
\begin{cases} K_{LP1}= \frac{p(C-Dj)^2}{(j-p)^2} \\  K_{LP2}= \frac{p(C-Dj)^2}{(j+p)^2} \end{cases}
$$

Since we're interested in the minimum balance of LP tokens, we keep only the second solution that will give the lowest balance possible. Last, we need to take the square root of $$K_{LP}$$ ​to get the actual LP token balance:

$$
Bal_{LPmin}= \sqrt{\frac{p(C-Dj)^2}{(j+p)^2}}=\sqrt{p} \frac{C-Dj}{j+p}
$$

Below is a calculation example:

{% embed url="https://docs.google.com/spreadsheets/d/1flg9LidXvxcAInw4-AtDCEFniMGoZg9fIgm-x36S3CA/edit#gid=0" %}

The spreadsheet is available here, make a copy to test your own parameters: [https://docs.google.com/spreadsheets/d/1flg9LidXvxcAInw4-AtDCEFniMGoZg9fIgm-x36S3CA/edit#gid=0](https://docs.google.com/spreadsheets/d/1flg9LidXvxcAInw4-AtDCEFniMGoZg9fIgm-x36S3CA/edit#gid=0)

