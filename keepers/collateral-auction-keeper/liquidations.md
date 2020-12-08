# Liquidations & Auctions

## Liquidations

The collateral keeper will liquidate all critical safes it encounters. This process starts new fixed discount collateral auctions:

```text
Liquidating ETH-A SAFE 0xc90F721DacfF8548777026253B8CB584426DC8C7 with locked_collateral=0.435000000000000000 liquidat
ion_price=194.362282878411910669975186104 generated_debt=85.069599661138521872 accumulatedRates=1.000357358433360132592605082
Checked 39 safes in 14 seconds
Sent transaction LiquidationEngine('0x4443d35BAEa0EeD30d9C7Fb136B8e507DC593646').liquidateSAFE('0x4554482d410000000000
00000000000000000000000000000000000000000000', '0xc90F721DacfF8548777026253B8CB584426DC8C7') with nonce=206, gas=434424, gas_price=30000000000 (tx_hash
=0x6688d6e94dc884ba294dd67626f1e0c749c76ca14fb7224b948b31a0e5286d6a)
Transaction LiquidationEngine('0x4443d35BAEa0EeD30d9C7Fb136B8e507DC593646').liquidateSAFE('0x4554482d41000000000000000000000000000000000000000000000000000000', '0xc90F721DacfF8548777026253B8CB584426DC8C7') was successful (tx_hash=0x6688d6e94dc884ba294dd67626f1e0c749c76ca14fb7224b948b31a0e5286d6a)
```

## Auctions

After an auction is started, the collateral keeper will be able to bid for and buy collateral at a discounted price.

```text
Started monitoring auction #8
Instantiated model using process '../models/collateral_model.sh --id 8 --collateral_auction_house 0xF8AAD33Cb9291Da4FF51377a6F1aB165c5E7Ab69'
Process '../models/collateral_model.sh --id 8 --collateral_auction_house 0xF8AAD33Cb9291Da4FF51377a6F1aB165c5E7Ab69' (pid #36) started
Checked auctions 0 to 8 in 0 seconds
Checking if internal system coin balance needs to be rebalanced
system coin token balance: 0.000000000000000000, SAFE Engine balance: 119.768970008628887937411195000000000000000000000
Sending new bid @284.538396004218362573 for auction 8
Sent transaction FixedDiscountCollateralAuctionHouse('0xF8AAD33Cb9291Da4FF51377a6F1aB165c5E7Ab69').buyCollateral(8, 4
318147966969218961926) with nonce=230, gas=303231, gas_price=30000000000 (tx_hash=0x2f63ae43a46ae777f31a0977363a4d1ebd5fb8486d68fd70987b199281c36e3a)
Transaction FixedDiscountCollateralAuctionHouse('0xF8AAD33Cb9291Da4FF51377a6F1aB165c5E7Ab69').buyCollateral(8, 431814
7966969218961926) was successful (tx_hash=0x2f63ae43a46ae777f31a0977363a4d1ebd5fb8486d68fd70987b199281c36e3a)
```

{% hint style="info" %}
**Bidding Gotchas**

1\) By default, the keeper submits a bid using all available system coin. This ensures the keeper gets the maximum amount of discounted collateral. 

2\) If there are multiple ongoing auctions, you might see this error until the first `buyCollateral` transaction is finished:

`Bid cost 5025.307970008628887936000000000000000000000000000 exceeds reservoir level of 0.000000000000000000411195000000000000000000000; bid will not be submitted`
{% endhint %}

## Swapping Bought Collateral for System Coins

The collateral keeper can swap collateral for system coin automatically on Uniswap when the collateral is exited from the system. This allows the keeper to be ready with system coin for the next collateral auction.To turn this option on, use this flag.

`--swap-collateral`

To set the max allowable slippage percent on Uniswap V2, set this flag:

`--max-swap-slippage <float>, default: 0.01`

## Exiting Collateral

By default, the keeper will periodically exit collateral \(that was bought from auctions\) from the system.  

To adjust the interval at which the keeper exits collateral, set:

`--return-collateral-interval, default:300 secs`

By default, the keeper will `exit`collateral when it's shutting down. 

To leave won collateral in the system on exit, specify this inside `run_auction_keeper.sh`:  
`--keep-collateral-in-safe-engine-on-exit`

```text
Exiting 0.344424752741987967 ETH-A from the SAFE Engine
Sent transaction <pyflex.gf.BasicCollateralJoin object at 0x7f0b7e92b5f8>.exit('0xdD1693BD8E307eCfDbe51D246562fc4109f871f8', 344424752741987967) with nonce=210, gas=153991, gas_price=30000000000 (tx_hash=0x88266148371a3dfea081fbc378c802ad02ef2bd9e0ccdfabfa2a53894dbffd7f)
Transaction <pyflex.gf.BasicCollateralJoin object at 0x7f0b7e92b5f8>.exit('0xdD1693BD8E307eCfDbe51D246562fc4109f871f8', 344424752741987967) was successful (tx_hash=0x88266148371a3dfea081fbc378c802ad02ef2bd9e0ccdfabfa2a53894dbffd7f)
Shutdown logic finished
Keeper terminated
```

