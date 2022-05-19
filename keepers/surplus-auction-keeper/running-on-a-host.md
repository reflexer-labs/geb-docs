---
description: Running a surplus auction-keeper on a host
---

# Running on a Host

{% hint style="info" %}
In order to participate in surplus auctions you need to bid with protocol tokens
{% endhint %}

## Prerequisites

Python 3.6+

### Clone:

```
git clone https://github.com/reflexer-labs/auction-keeper.git
cd auction-keeper
git submodule update --init --recursive
```

### Install:

This creates a virtual environment and installs requirements:

`./install.sh`

## 1. Start virtualenv

`source _virtualenv/bin/activate`

## 2. Modify model file as needed

A basic surplus auction bidding model can be found in `models/surplus_model.py`. This model retrieves the latest FLX/USD price from coingecko and will automatically place bids in an auction.

You probably want to modify the following variables in `models/surplus_model.py`:

`STARTING_FLX_MULTIPLIER`: The maximum acceptable FLX price to use when bidding. This value will be used when bidding on a new auction with no previous bids. Default: `1.50` meaning the maxiimum price to accept for FLX(in RAI) is 150% of the current FLX/USD market price

`MINIMUM_FLX_MULTIPLIER`: The minimum acceptable FLX price to use when bidding. Default: `1.10` meaning the minimum price to accept for FLX(in RAI) is 110% of the current FLX/USD market price

`MY_BID_INCREASE`: The amount of bid increase(in FLX) to make when outbidding another bidder. If value is less than the auction house' `bidIncrease`, then it will use the auction house setting. Example: A value of `1.10` will create bid increases of 10%. Note: Current `bidIncrease` on mainnet is `1.03`. Default: `1.03`

### Ensure script is executable

`chmod +x surplus_model.py`

For more information about bidding models, see [Bidding Models](../BiddingModels.md)

## 3. Modify keeper run file

Modify the following variables in `run_surplus_keeper_host.sh`

`KEEPER_ADDRESS` - the keeper's address. It should be in checksummed format (not lowercase).

`ETH_RPC_URL` - the URL of your ethereum RPC connection

`KEYSTORE_FILE` - your Ethereum UTC JSON keystore filename

For more information about this keystore format and how to generate them, check:

* [Ethereum UTC / JSON Wallet Encryption](https://wizardforcel.gitbooks.io/practical-cryptography-for-developers-book/content/symmetric-key-ciphers/ethereum-wallet-encryption.html)
* [keythereum](https://github.com/ethereumjs/keythereum)

`GAS_MAXIMUM` -maximum gas price, in GWEI

### Ensure script is executable

`chmod +x run_surplus_keeper_host.sh`

## 4. Start the keeper and enter your keystore file password

`./run_surplus_keeper_host.sh`

```
$ ./run_surplus_keeper_host.sh
Password for /keystore/key.json:
```

## Surplus Auction Output

#### Start a Surplus Auction

If enough surplus exists in the system, the surplus _auction-keeper_ will call `auctionSurplus` to start a surplus auction.

```
2021-01-19 21:15:20,644 INFO     Initiating a surplus auction with coin balance=857.460939042493680361118426229210746756033227844
2021-01-19 21:15:21,317 INFO     Sent transaction AccountingEngine('0x73EC2a627655134886477D10A41275f54556E0e2').auctionSurplus() with nonce=771, gas=290341, gas_price=3000000000 (tx_hash=0x67fdeda8c2dedf5ba05cecf4184e38f3f0353c750a5f466ebf340730e0aa330e)
```

#### Bid on a Surplus Auction

If the surplus _auction-keeper_ has FLX, it will bid on surplus auctions by calling `increaseBidSize`

```
2021-01-19 21:15:33,578 INFO     Sending new bid @100.000000000000000000 for auction 16
2021-01-19 21:15:33,688 INFO     Sent transaction PreSettlementSurplusAuctionHouse('0xE04ccD802E5e37bE1A64036ce8E7e514E4DBE475').increaseBidSize(16, 2000000000000000000000000000000000000000000000, 20000000000000000) with nonce=773, gas=214285, gas_price=3000000000
```

#### Full Log Output

```
2021-01-19 21:15:07,178 INFO     Keeper connected to RPC connection https://myparitynode.com
2021-01-19 21:15:07,179 INFO     Keeper operating as 0xdD1693BD8E307eCfDbe51D246562fc4109f871f8
2021-01-19 21:15:07,203 INFO     Executing keeper startup logic
2021-01-19 21:15:08,241 INFO     Checking if internal system coin balance needs to be rebalanced
2021-01-19 21:15:08,274 INFO     Joining 1000.613947077334748390 system coin to the SAFE Engine
2021-01-19 21:15:08,334 INFO     Sent transaction <pyflex.gf.CoinJoin object at 0x7fed8cc27208>.join('0xdD1693BD8E307eCfDbe51D246562fc4109f871f8', 10006139470773347483
90) with nonce=770, gas=159975, gas_price=3000000000 (tx_hash=0x996585d78c2ee40536cb575be0492c888e1603dad01b571ec805ad5e3bde231f)
2021-01-19 21:15:12,496 INFO     Transaction <pyflex.gf.CoinJoin object at 0x7fed8cc27208>.join('0xdD1693BD8E307eCfDbe51D246562fc4109f871f8', 1000613947077334748390) w
as successful (tx_hash=0x996585d78c2ee40536cb575be0492c888e1603dad01b571ec805ad5e3bde231f)
2021-01-19 21:15:12,506 INFO     Prot balance is 4.800000000000000000
2021-01-19 21:15:12,507 INFO     Keeper will perform the following operation(s) in parallel:
2021-01-19 21:15:12,507 INFO     --> Check thresholds in Accounting Engine Contract and start new surplus auctions once reached
2021-01-19 21:15:12,507 INFO     --> Check all auctions being monitored and evaluate bidding opportunity every 4.0 seconds
2021-01-19 21:15:12,508 INFO     --> Check all auctions and settle for 0xdD1693BD8E307eCfDbe51D246562fc4109f871f8
2021-01-19 21:15:12,511 INFO     Keeper will use Node gas price (currently 3.0 Gwei, changes over time) and will multiply by 1.125 every 30s to a maximum of 2000.0 Gwe
i for transactions and bids unless model instructs otherwise
2021-01-19 21:15:12,512 INFO     Watching for new blocks
2021-01-19 21:15:12,514 INFO     Started 2 timer(s)
2021-01-19 21:15:20,644 INFO     Initiating a surplus auction with coin balance=857.460939042493680361118426229210746756033227844
2021-01-19 21:15:21,317 INFO     Sent transaction AccountingEngine('0x73EC2a627655134886477D10A41275f54556E0e2').auctionSurplus() with nonce=771, gas=290341, gas_price=3000000000 (tx_hash=0x67fdeda8c2dedf5ba05cecf4184e38f3f0353c750a5f466ebf340730e0aa330e)
2021-01-19 21:15:29,370 INFO     Transaction AccountingEngine('0x73EC2a627655134886477D10A41275f54556E0e2').auctionSurplus() was successful (tx_hash=0x67fdeda8c2dedf5ba05cecf4184e38f3f0353c750a5f466ebf340730e0aa330e)
2021-01-19 21:15:29,641 INFO     Started monitoring auction #16
2021-01-19 21:15:29,642 INFO     Instantiated model using process '/models/surplus_model.sh --id 16 --surplus_auction_house 0xE04ccD802E5e37bE1A64036ce8E7e514E4DBE475'
2021-01-19 21:15:29,653 INFO     Process '/models/surplus_model.sh --id 16 --surplus_auction_house 0xE04ccD802E5e37bE1A64036ce8E7e514E4DBE475' (pid #29) started
2021-01-19 21:15:29,675 INFO     Checked auctions 0 to 16 in 0 seconds
2021-01-19 21:15:32,738 INFO     Initiating a surplus auction with coin balance=855.460939042493680361118426229210746756033227844
2021-01-19 21:15:33,430 INFO     Sent transaction AccountingEngine('0x73EC2a627655134886477D10A41275f54556E0e2').auctionSurplus() with nonce=772, gas=275341, gas_price=3000000000 (tx_hash=0xe00e3bbeeb12c23d13c52a34bbf2f5457d2ea9ae513baffd8d65133adba7b19d)
2021-01-19 21:15:33,578 INFO     Sending new bid @100.000000000000000000 for auction 16
2021-01-19 21:15:33,688 INFO     Sent transaction PreSettlementSurplusAuctionHouse('0xE04ccD802E5e37bE1A64036ce8E7e514E4DBE475').increaseBidSize(16, 2000000000000000000000000000000000000000000000, 20000000000000000) with nonce=773, gas=214285, gas_price=3000000000 (tx_hash=0x1a6e219ba11bc300e671703ae73ff5cc7f36e4be0d56fd7751c4bb24a3628106)
2021-01-19 21:15:41,040 INFO     Transaction AccountingEngine('0x73EC2a627655134886477D10A41275f54556E0e2').auctionSurplus() was successful (tx_hash=0xe00e3bbeeb12c23d13c52a34bbf2f5457d2ea9ae513baffd8d65133adba7b19d)
```
