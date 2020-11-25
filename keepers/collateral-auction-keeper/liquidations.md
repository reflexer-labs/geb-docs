# Liquidations

```text
Liquidating ETH-A SAFE 0xc90F721DacfF8548777026253B8CB584426DC8C7 with locked_collateral=0.435000000000000000 liquidat
ion_price=194.362282878411910669975186104 generated_debt=85.069599661138521872 accumulatedRates=1.000357358433360132592605082
Checked 39 safes in 14 seconds
Sent transaction LiquidationEngine('0x4443d35BAEa0EeD30d9C7Fb136B8e507DC593646').liquidateSAFE('0x4554482d410000000000
00000000000000000000000000000000000000000000', '0xc90F721DacfF8548777026253B8CB584426DC8C7') with nonce=206, gas=434424, gas_price=30000000000 (tx_hash
=0x6688d6e94dc884ba294dd67626f1e0c749c76ca14fb7224b948b31a0e5286d6a)
Transaction LiquidationEngine('0x4443d35BAEa0EeD30d9C7Fb136B8e507DC593646').liquidateSAFE('0x4554482d41000000000000000000000000000000000000000000000000000000', '0xc90F721DacfF8548777026253B8CB584426DC8C7') was successful (tx_hash=0x6688d6e94dc884ba294dd67626f1e0c749c76ca14fb7224b948b31a0e5286d6a)
```

`--return-collateral-interval`

  
`--keep-collateral-in-safe-engine-on-exit`

```text
Started monitoring auction #8
Instantiated model using process '../models/collateral_model.sh --id 8 --collateral_auction_house 0xF8AAD33Cb9291Da4FF51377a6F1aB165c5E7Ab69'
Process '../models/collateral_model.sh --id 8 --collateral_auction_house 0xF8AAD33Cb9291Da4FF51377a6F1aB165c5E7Ab69' (pid #36) started
Checked auctions 0 to 8 in 0 seconds
Checking if internal system coin balance needs to be rebalanced
system coin token balance: 0.000000000000000000, SAFE Engine balance: 119.768970008628887937411195000000000000000000000
Sent transaction FixedDiscountCollateralAuctionHouse('0xF8AAD33Cb9291Da4FF51377a6F1aB165c5E7Ab69').getCollateralBought(8, 119768970008628887937) with nonce=207, gas=163729, gas_price=default (tx_hash=0x567970f3ec40187bca3a2d65e4781ca04158196b3ae93fa46da10c7418ea1b29)
```

## Swapping Collateral for system coin

`--swap-collateral`

`--max-swap-slippage <float>, default: 0.01`  


#### Exiting Collateral

```text
Exiting 0.344424752741987967 ETH-A from the SAFE Engine
Sent transaction <pyflex.gf.BasicCollateralJoin object at 0x7f0b7e92b5f8>.exit('0xdD1693BD8E307eCfDbe51D246562fc4109f871f8', 344424752741987967) with nonce=210, gas=153991, gas_price=30000000000 (tx_hash=0x88266148371a3dfea081fbc378c802ad02ef2bd9e0ccdfabfa2a53894dbffd7f)
Transaction <pyflex.gf.BasicCollateralJoin object at 0x7f0b7e92b5f8>.exit('0xdD1693BD8E307eCfDbe51D246562fc4109f871f8', 344424752741987967) was successful (tx_hash=0x88266148371a3dfea081fbc378c802ad02ef2bd9e0ccdfabfa2a53894dbffd7f)
Shutdown logic finished
Keeper terminated
```

