# Curve V1 Savior Details

The Curve V1 savior require to deposit Curve RAI/Tripool LP tokens. The contract of the pools is: [https://etherscan.io/address/0x618788357D0EBd8A37e763ADab3bc575D54c2C7d](https://etherscan.io/address/0x618788357D0EBd8A37e763ADab3bc575D54c2C7d)

Upon save, the Curve savior will withdraw all underlying liquidity RAI + 3pool token of the LP share. It will then use the RAI proceeds to pay back as much debt as possible of the Safe. The 3pool tokens aren't used by the savior and will need to be withdrawn manually from the Savior.

### Minimum savior balance calculation

We reuse the variable naming and debt/collateral liquidation relationship from the [Uni-v2 savior calculation](https://docs.reflexer.finance/liquidation-protection/uni-v2-rai-eth-savior-math#minimum-savior-balance-formula).&#x20;

$$
\frac{C + C_{lp}}{D - D_{lp}}  = \frac{A_{cc}R_{tar}P_{rp}}{P_{liq}}
$$

In the curve savior we aren't adding any collateral, therefore $$C_{lp}=0$$. For$$D_{lp}$$ we have to calculate the amount of RAI withdrawn from the pool. We will assume that the USD value of the pool stays 50/50 and that RAI trades close to it redemption price. Therefore for each RAI-3Pool LP token $$K_{RAI3Pool}$$ we are getting the following amount of RAI $$D = \frac{K_{RAI3Pool}}{2 P_{rp}}$$which give us the following formula:

$$
\frac{2CP_{rp}}{2P_{rp}D - K_{RAI3Pool}} = \frac{A_{cc}R_{tar}P_{rp}}{P_{liq}}
$$

&#x20;Which give us after also replacing the liquidation price:

$$
K = 2P_{rp}D- \frac{2CP_{liq}}{A_{acc}R_{tar}} =  2P_{rp}D-\frac{2DP_{rp}R_{liq}}{R_{tar}} = 2P_{rp}D( 1 - \frac{R_{liq}}{R_{tar}})
$$

To this we have to add the $2000 Keeper fee. To help with the calculation, use this spreadsheet.&#x20;

{% embed url="https://docs.google.com/spreadsheets/d/1GeY8B6RW01GeQiM1FIFBIlgA5bV6Qdzv1XsvYKwVU4Y/edit#gid=0" %}
