# Curve V1 Savior Details

**This is the math and logic that would apply in the case of a Curve V1 saviour integrated with the RAI protocol.**

### Overview

The Curve V1 savior requires SAFE owners to deposit Curve RAI/3Pool LP tokens. The RAI/3Pool contract can be found [here](https://etherscan.io/address/0x618788357D0EBd8A37e763ADab3bc575D54c2C7d).

Upon liquidation, the Curve savior will withdraw all underlying liquidity (RAI + 3Pool tokens) from the LP shares protecting the liquidated SAFE. It will then use the withdrawn RAI to pay back as much debt as possible from the SAFE. The 3Pool tokens aren't used in this process and will need to be withdrawn manually from the savior at a later date.

### Minimum Savior Balance Calculation

We reuse the variable naming and debt/collateral liquidation relationship from the [Uni-v2 savior calculation](https://docs.reflexer.finance/liquidation-protection/uni-v2-rai-eth-savior-math#minimum-savior-balance-formula).&#x20;

$$
\frac{C + C_{lp}}{D - D_{lp}}  = \frac{A_{cc}R_{tar}P_{rp}}{P_{liq}}
$$

In the curve savior we aren't adding any collateral, therefore $$C_{lp}=0$$. For$$D_{lp}$$ we have to calculate the amount of RAI withdrawn from the pool. We will assume that the USD value of the pool stays 50/50 and that RAI trades close to it redemption price. Therefore for each RAI-3Pool LP token $$K_{RAI3Pool}$$ we are getting $$D = \frac{K_{RAI3Pool}}{2 P_{rp}}$$RAI which give us the following formula:

$$
\frac{2CP_{rp}}{2P_{rp}D - K_{RAI3Pool}} = \frac{A_{cc}R_{tar}P_{rp}}{P_{liq}}
$$

&#x20;Which then gives us (after also replacing the liquidation price):

$$
K = 2P_{rp}D- \frac{2CP_{liq}}{A_{acc}R_{tar}} =  2P_{rp}D-\frac{2DP_{rp}R_{liq}}{R_{tar}} = 2P_{rp}D( 1 - \frac{R_{liq}}{R_{tar}})
$$

To this we have to add the $2000 liquidator fee.&#x20;

To help with the whole calculation, you can use the following spreadsheet:

{% embed url="https://docs.google.com/spreadsheets/d/1GeY8B6RW01GeQiM1FIFBIlgA5bV6Qdzv1XsvYKwVU4Y/edit#gid=0" %}
