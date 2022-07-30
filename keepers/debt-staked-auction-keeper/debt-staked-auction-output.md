---
description: >-
  Below you will see the debt staked auction-keeper start and bid on a debt
  staked auction.
---

# Debt Staked Auction Output

.Pop debt from queue

If debt from collateral auctions still exists after `AccountingEngine.pop_debt_delay()`, the debt staked _auction-keeper_ will call `popDebtFromQueue()`

```
2021-01-20 04:16:22,462 INFO     Sent transaction AccountingEngine('0x73EC2a627655134886477D10A41275f54556E0e2').popDebtFromQueue(1611116032) with nonce=1519, gas=159390, gas_price=10000000000 (tx_hash=0x7266984842ce52d8df52a775d889bb7040e0bac742d51f369e0c23e3e89dd560)
```

#### Settling debt

Before starting a debt auction, the keeper will use any surplus to offset any existing debt. It does this by calling `settleDebt()`

```
2022-07-11 20:49:06,502 INFO     Settling debt. coin_balance=0.003849105356317367507721355548000000000000000 unqueued_enauctioned_debt=34709.319942457639191758056217752288276396399199497
2022-07-11 20:49:06,798 INFO     Sent transaction AccountingEngine('0x6073E8FE874B53732b5DdD469a2De4047f33C64B').settleDebt(3849105356317367507721355548000000000000000) with nonce=5502, gas=160806, gas_price=2500000007 (tx_hash=0x0cbf049a593cca6eaee174bb19be503736adc0562bf5837b4f2acb3e3dc37435)
2022-07-11 20:49:08,855 INFO     Transaction AccountingEngine('0x6073E8FE874B53732b5DdD469a2De4047f33C64B').settleDebt(3849105356317367507721355548000000000000000) was successful (tx_hash=0x0cbf049a593cca6eaee174bb19be503736adc0562bf5837b4f2acb3e3dc37435)
```

#### Starting a Debt Staked Auction

Finally, if debt still exists and is enough to start an auction, the _auction-keeper_ will call `auctionAncestorTokens()`

```
2022-07-11 20:49:09,467 INFO     Sent transaction <pyflex.gf.GebStaking object at 0x7f7bf8ddc190>.auctionAncestorTokens() with nonce=5503, gas=428111, gas_price=2500000007 (tx_hash=0xba5e1523fc60666010554b644a69e826375a28ed393a2498982db0eb2097e751)
2022-07-11 20:49:12,900 INFO     Transaction <pyflex.gf.GebStaking object at 0x7f7bf8ddc190>.auctionAncestorTokens() was successful (tx_hash=0xba5e1523fc60666010554b644a69e826375a28ed393a2498982db0eb2097e751)
```

#### Bidding on a Debt Staked Auction

To bid on debt staked auctions, the _auction-keeper_ calls `increaseBidSize()`

```
2022-07-11 20:49:13,106 INFO     Started monitoring auction #11
2022-07-11 20:49:13,106 INFO     Instantiated model using process '/models/debt_staked_model.py --id 11 --staked_token_auction_house 0x328BCbF2d4c2D78936f52172c741456136FA7e92'
2022-07-11 20:49:13,115 INFO     Process '/models/debt_staked_model.py --id 11 --staked_token_auction_house 0x328BCbF2d4c2D78936f52172c741456136FA7e92' (pid #23) started
2022-07-11 20:49:13,204 INFO     Feeding auction 11 model input {'id': '11', 'bid_amount': '0.000000000000000000000000001000000000000000000', 'amount_to_sell': '0.001000000000000000', 'block_time': 1657572552, 'auction_deadline': 1657573452, 'price': '0.000000000000000000', 'bid_increase': '1.050000000000000000', 'high_bidder': '0x0000000000000000000000000000000000000000', 'staked_token_auction_house': '0x328BCbF2d4c2D78936f52172c741456136FA7e92'}
2022-07-11 20:49:13,238 INFO     Checked auctions 11 to 11 in 0 seconds
2022-07-11 20:49:26,756 INFO     Sending new bid @0.256000000000000000 for auction 11
2022-07-11 20:49:26,965 INFO     Sent transaction StakedTokenAuctionHouse('0x328BCbF2d4c2D78936f52172c741456136FA7e92').increaseBidSize(11, 1000000000000000, 1283356467456783863967795170000000000000000) with nonce=5504, gas=182102, gas_price=2500000007 (tx_hash=0x6b9c65ebd0b55090af120543d4024d095059ddf66542af997e1fb6543fa1e22c)
2022-07-11 20:49:29,322 INFO     Transaction StakedTokenAuctionHouse('0x328BCbF2d4c2D78936f52172c741456136FA7e92').increaseBidSize(11, 1000000000000000, 1283356467456783863967795170000000000000000) was successful (tx_hash=0x6b9c65ebd0b55090af120543d4024d095059ddf66542af997e1fb6543fa1e22c)
```

#### Settling a Debt Staked Auction

To settle a debt staked auctions, the _auction-keeper_ calls `settleAuction()`

```
2022-07-11 20:59:51,693 INFO     Sent transaction StakedTokenAuctionHouse('0x328BCbF2d4c2D78936f52172c741456136FA7e92').settleAuction(11) with nonce=55
07, gas=227887, gas_price=2500000007 (tx_hash=0x4e1d347341b9e4e1567aff93585db3fb7529b024a8282ffa6d1da2164d7264f3)
2022-07-11 20:59:52,715 INFO     Transaction StakedTokenAuctionHouse('0x328BCbF2d4c2D78936f52172c741456136FA7e92').settleAuction(11) was successful (tx
_hash=0x4e1d347341b9e4e1567aff93585db3fb7529b024a8282ffa6d1da2164d7264f3)
```

### Full Log Output

```
[ec2-user@ip-172-31-40-135 ~]$  ./run_debt_staked_keeper.sh 
Password for /keystore/keystore.json: 
2022-07-11 20:48:43,736 INFO     Keeper connected to RPC connection https://eth-kovan.alchemyapi.io/v2blahblahblah/
2022-07-11 20:48:43,736 INFO     Keeper operating as 0xdD1693BD8E307eCfDbe51D246562fc4109f871f8
2022-07-11 20:48:43,928 INFO     Executing keeper startup logic
2022-07-11 20:48:45,427 INFO     Keeper will perform the following operation(s) in parallel:
2022-07-11 20:48:45,427 INFO     --> Check thresholds in Accounting Engine Contract and start new debt_staked auctions once reached
2022-07-11 20:48:45,427 INFO     --> Check all auctions being monitored and evaluate bidding opportunity every 20.0 seconds
2022-07-11 20:48:45,427 INFO     --> Check all auctions and settle for any address
2022-07-11 20:48:45,471 INFO     Keeper will use Node gas price (currently 2.5 Gwei, changes over time) with initial multiplier 1.0 and will multiply by 1.125 every 42s to a maximum of 200.0 Gwei for transactions and bids unless model instructs otherwise
2022-07-11 20:48:45,622 INFO     Watching for new blocks
2022-07-11 20:48:45,624 INFO     Started 2 timer(s)
2022-07-11 20:49:06,502 INFO     Settling debt. coin_balance=0.003849105356317367507721355548000000000000000 unqueued_enauctioned_debt=34709.319942457639191758056217752288276396399199497
2022-07-11 20:49:06,798 INFO     Sent transaction AccountingEngine('0x6073E8FE874B53732b5DdD469a2De4047f33C64B').settleDebt(3849105356317367507721355548000000000000000) with nonce=5502, gas=160806, gas_price=2500000007 (tx_hash=0x0cbf049a593cca6eaee174bb19be503736adc0562bf5837b4f2acb3e3dc37435)
2022-07-11 20:49:08,855 INFO     Transaction AccountingEngine('0x6073E8FE874B53732b5DdD469a2De4047f33C64B').settleDebt(3849105356317367507721355548000000000000000) was successful (tx_hash=0x0cbf049a593cca6eaee174bb19be503736adc0562bf5837b4f2acb3e3dc37435)
2022-07-11 20:49:09,467 INFO     Sent transaction <pyflex.gf.GebStaking object at 0x7f7bf8ddc190>.auctionAncestorTokens() with nonce=5503, gas=428111, gas_price=2500000007 (tx_hash=0xba5e1523fc60666010554b644a69e826375a28ed393a2498982db0eb2097e751)
2022-07-11 20:49:12,900 INFO     Transaction <pyflex.gf.GebStaking object at 0x7f7bf8ddc190>.auctionAncestorTokens() was successful (tx_hash=0xba5e1523fc60666010554b644a69e826375a28ed393a2498982db0eb2097e751)
2022-07-11 20:49:13,105 INFO     Input for auction 11: {'id': '11', 'bid_amount': '0.000000000000000000000000001000000000000000000', 'amount_to_sell': '0.001000000000000000', 'block_time': 1657572552, 'auction_deadline': 1657573452, 'price': '0.000000000000000000', 'bid_increase': '1.050000000000000000', 'high_bidder': '0x0000000000000000000000000000000000000000', 'staked_token_auction_house': '0x328BCbF2d4c2D78936f52172c741456136FA7e92'}
2022-07-11 20:49:13,106 INFO     Auction 11 deleted: False
2022-07-11 20:49:13,106 INFO     Started monitoring auction #11
2022-07-11 20:49:13,106 INFO     Instantiated model using process '/models/debt_staked_model.py --id 11 --staked_token_auction_house 0x328BCbF2d4c2D78936f52172c741456136FA7e92'
2022-07-11 20:49:13,115 INFO     Process '/models/debt_staked_model.py --id 11 --staked_token_auction_house 0x328BCbF2d4c2D78936f52172c741456136FA7e92' (pid #23) started
2022-07-11 20:49:13,204 INFO     Feeding auction 11 model input {'id': '11', 'bid_amount': '0.000000000000000000000000001000000000000000000', 'amount_to_sell': '0.001000000000000000', 'block_time': 1657572552, 'auction_deadline': 1657573452, 'price': '0.000000000000000000', 'bid_increase': '1.050000000000000000', 'high_bidder': '0x0000000000000000000000000000000000000000', 'staked_token_auction_house': '0x328BCbF2d4c2D78936f52172c741456136FA7e92'}
2022-07-11 20:49:13,238 INFO     Checked auctions 11 to 11 in 0 seconds
2022-07-11 20:49:26,756 INFO     Sending new bid @0.256000000000000000 for auction 11
2022-07-11 20:49:26,965 INFO     Sent transaction StakedTokenAuctionHouse('0x328BCbF2d4c2D78936f52172c741456136FA7e92').increaseBidSize(11, 1000000000000000, 1283356467456783863967795170000000000000000) with nonce=5504, gas=182102, gas_price=2500000007 (tx_hash=0x6b9c65ebd0b55090af120543d4024d095059ddf66542af997e1fb6543fa1e22c)
2022-07-11 20:49:27,084 INFO     Sent transaction <pyflex.gf.GebStaking object at 0x7f7bf8ddc190>.auctionAncestorTokens() with nonce=5505, gas=393911, gas_price=2500000007 (tx_hash=0x191649561ef51dfee298f397e0234c5ad10ecf388416328f965bbcd556ff0e78)
2022-07-11 20:49:29,322 INFO     Transaction StakedTokenAuctionHouse('0x328BCbF2d4c2D78936f52172c741456136FA7e92').increaseBidSize(11, 1000000000000000, 1283356467456783863967795170000000000000000) was successful (tx_hash=0x6b9c65ebd0b55090af120543d4024d095059ddf66542af997e1fb6543fa1e22c)
2022-07-11 20:49:29,439 INFO     Transaction <pyflex.gf.GebStaking object at 0x7f7bf8ddc190>.auctionAncestorTokens() was successful (tx_hash=0x191649561ef51dfee298f397e0234c5ad10ecf388416328f965bbcd556ff0e78)
2022-07-11 20:49:29,619 INFO     Input for auction 11: {'id': '11', 'bid_amount': '0.001283356467456783863967795170000000000000000', 'amount_to_sell': '0.001000000000000000', 'block_time': 1657572568, 'auction_deadline': 1657573452, 'price': '0.255999805335068288', 'bid_increase': '1.050000000000000000', 'high_bidder': '0xdD1693BD8E307eCfDbe51D246562fc4109f871f8', 'bid_expiry': '1657573168', 'staked_token_auction_house': '0x328BCbF2d4c2D78936f52172c741456136FA7e92'}
2022-07-11 20:49:29,619 INFO     Auction 11 deleted: False
2022-07-11 20:49:29,768 INFO     Input for auction 12: {'id': '12', 'bid_amount': '0.000000000000000000000000001000000000000000000', 'amount_to_sell': '0.001000000000000000', 'block_time': 1657572568, 'auction_deadline': 1657573468, 'price': '0.000000000000000000', 'bid_increase': '1.050000000000000000', 'high_bidder': '0x0000000000000000000000000000000000000000', 'staked_token_auction_house': '0x328BCbF2d4c2D78936f52172c741456136FA7e92'}
2022-07-11 20:49:29,768 INFO     Auction 12 deleted: False
2022-07-11 20:49:29,768 WARNING  Processing auctions [11]; ignoring [12]
2022-07-11 20:49:29,798 INFO     Checked auctions 11 to 12 in 0 seconds
2022-07-11 20:49:47,145 INFO     Sent transaction <pyflex.gf.GebStaking object at 0x7f7bf8ddc190>.auctionAncestorTokens() with nonce=5506, gas=393911, 
gas_price=2500000007 (tx_hash=0x03e820d3536bce35eaf6f22f51d8132499bb6695ac16732c31ad1ff65f48a29e)
2022-07-11 20:49:48,861 INFO     Transaction <pyflex.gf.GebStaking object at 0x7f7bf8ddc190>.auctionAncestorTokens() was successful (tx_hash=0x03e820d3
536bce35eaf6f22f51d8132499bb6695ac16732c31ad1ff65f48a29e)
2022-07-11 20:49:49,058 INFO     Input for auction 11: {'id': '11', 'bid_amount': '0.001283356467456783863967795170000000000000000', 'amount_to_sell': '0.001000000000000000', 'block_time': 1657572588, 'auction_deadline': 1657573452, 'price': '0.255998832012630103', 'bid_increase': '1.050000000000000000', 'high_bidder': '0xdD1693BD8E307eCfDbe51D246562fc4109f871f8', 'bid_expiry': '1657573168', 'staked_token_auction_house': '0x328BCbF2d4c2D78936f52172c741456136FA7e92'}
2022-07-11 20:49:49,058 INFO     Auction 11 deleted: False
2022-07-11 20:49:49,220 INFO     Input for auction 12: {'id': '12', 'bid_amount': '0.000000000000000000000000001000000000000000000', 'amount_to_sell': '0.001000000000000000', 'block_time': 1657572588, 'auction_deadline': 1657573468, 'price': '0.000000000000000000', 'bid_increase': '1.050000000000000000', 'high_bidder': '0x0000000000000000000000000000000000000000', 'staked_token_auction_house': '0x328BCbF2d4c2D78936f52172c741456136FA7e92'}
2022-07-11 20:49:49,220 INFO     Auction 12 deleted: False
2022-07-11 20:49:49,361 INFO     Input for auction 13: {'id': '13', 'bid_amount': '0.000000000000000000000000001000000000000000000', 'amount_to_sell': '0.001000000000000000', 'block_time': 1657572588, 'auction_deadline': 1657573488, 'price': '0.000000000000000000', 'bid_increase': '1.050000000000000000', 'high_bidder': '0x0000000000000000000000000000000000000000', 'staked_token_auction_house': '0x328BCbF2d4c2D78936f52172c741456136FA7e92'}
2022-07-11 20:49:49,361 INFO     Auction 13 deleted: False
2022-07-11 20:49:49,361 WARNING  Processing auctions [11]; ignoring [12, 13]
2022-07-11 20:49:49,400 INFO     Checked auctions 11 to 13 in 0 seconds
2022-07-11 20:50:06,979 INFO     Can't start staked_token auction as there are already 3 active auctions
2022-07-11 20:50:07,179 INFO     Input for auction 11: {'id': '11', 'bid_amount': '0.001283356467456783863967795170000000000000000', 'amount_to_sell': '0.001000000000000000', 'block_time': 1657572604, 'auction_deadline': 1657573452, 'price': '0.255998053357343998', 'bid_increase': '1.050000000000000000', 'high_bidder': '0xdD1693BD8E307eCfDbe51D246562fc4109f871f8', 'bid_expiry': '1657573168', 'staked_token_auction_house': '0x328BCbF2d4c2D78936f52172c741456136FA7e92'}
2022-07-11 20:50:07,179 INFO     Auction 11 deleted: False
2022-07-11 20:50:07,316 INFO     Input for auction 12: {'id': '12', 'bid_amount': '0.000000000000000000000000001000000000000000000', 'amount_to_sell': '0.001000000000000000', 'block_time': 1657572604, 'auction_deadline': 1657573468, 'price': '0.000000000000000000', 'bid_increase': '1.050000000000000000', 'high_bidder': '0x0000000000000000000000000000000000000000', 'staked_token_auction_house': '0x328BCbF2d4c2D78936f52172c741456136FA7e92'}
2022-07-11 20:50:07,316 INFO     Auction 12 deleted: False
2022-07-11 20:50:07,467 INFO     Input for auction 13: {'id': '13', 'bid_amount': '0.000000000000000000000000001000000000000000000', 'amount_to_sell': '0.001000000000000000', 'block_time': 1657572604, 'auction_deadline': 1657573488, 'price': '0.000000000000000000', 'bid_increase': '1.050000000000000000', 'high_bidder': '0x0000000000000000000000000000000000000000', 'staked_token_auction_house': '0x328BCbF2d4c2D78936f52172c741456136FA7e92'}
2022-07-11 20:50:07,467 INFO     Auction 13 deleted: False
2022-07-11 20:50:07,467 WARNING  Processing auctions [11]; ignoring [12, 13]
2022-07-11 20:50:07,504 INFO     Checked auctions 11 to 13 in 0 seconds
2022-07-11 20:50:27,228 INFO     Can't start staked_token auction as there are already 3 active auctions
2022-07-11 20:50:27,397 INFO     Input for auction 11: {'id': '11', 'bid_amount': '0.001283356467456783863967795170000000000000000', 'amount_to_sell': '0.001000000000000000', 'block_time': 1657572624, 'auction_deadline': 1657573452, 'price': '0.255997080041566909', 'bid_increase': '1.050000000000000000', 'high_bidder': '0xdD1693BD8E307eCfDbe51D246562fc4109f871f8', 'bid_expiry': '1657573168', 'staked_token_auction_house': '0x328BCbF2d4c2D78936f52172c741456136FA7e92'}
2022-07-11 20:50:27,397 INFO     Auction 11 deleted: False
2022-07-11 20:50:27,578 INFO     Input for auction 12: {'id': '12', 'bid_amount': '0.000000000000000000000000001000000000000000000', 'amount_to_sell': '0.001000000000000000', 'block_time': 1657572624, 'auction_deadline': 1657573468, 'price': '0.000000000000000000', 'bid_increase': '1.050000000000000000', 'high_bidder': '0x0000000000000000000000000000000000000000', 'staked_token_auction_house': '0x328BCbF2d4c2D78936f52172c741456136FA7e92'}
2022-07-11 20:50:27,578 INFO     Auction 12 deleted: False
2022-07-11 20:50:27,730 INFO     Input for auction 13: {'id': '13', 'bid_amount': '0.000000000000000000000000001000000000000000000', 'amount_to_sell': '0.001000000000000000', 'block_time': 1657572624, 'auction_deadline': 1657573488, 'price': '0.000000000000000000', 'bid_increase': '1.050000000000000000', 'high_bidder': '0x0000000000000000000000000000000000000000', 'staked_token_auction_house': '0x328BCbF2d4c2D78936f52172c741456136FA7e92'}
2022-07-11 20:50:27,731 INFO     Auction 13 deleted: False
2022-07-11 20:50:27,731 WARNING  Processing auctions [11]; ignoring [12, 13]
2022-07-11 20:50:27,772 INFO     Checked auctions 11 to 13 in 0 seconds


2022-07-11 20:59:51,693 INFO     Sent transaction StakedTokenAuctionHouse('0x328BCbF2d4c2D78936f52172c741456136FA7e92').settleAuction(11) with nonce=55
07, gas=227887, gas_price=2500000007 (tx_hash=0x4e1d347341b9e4e1567aff93585db3fb7529b024a8282ffa6d1da2164d7264f3)
2022-07-11 20:59:52,715 INFO     Transaction StakedTokenAuctionHouse('0x328BCbF2d4c2D78936f52172c741456136FA7e92').settleAuction(11) was successful (tx
_hash=0x4e1d347341b9e4e1567aff93585db3fb7529b024a8282ffa6d1da2164d7264f3)
2022-07-11 20:59:52,995 INFO     Terminating model using process '/models/debt_staked_model.py --id 11 --staked_token_auction_house 0x328BCbF2d4c2D7893
6f52172c741456136FA7e92'
2022-07-11 20:59:52,995 INFO     Stopped monitoring auction #11 as it's not active anymore
2022-07-11 20:59:52,999 INFO     Process '/models/debt_staked_model.py --id 11 --staked_token_auction_house 0x328BCbF2d4c2D78936f52172c741456136FA7e92'
 (pid #23) terminated


2022-07-11 21:05:01,357 INFO     Auction 13 ended without bids; resurrecting auction
2022-07-11 21:05:01,562 INFO     Sent transaction StakedTokenAuctionHouse('0x328BCbF2d4c2D78936f52172c741456136FA7e92').restartAuction(13) with nonce=5515, gas=142873, gas_price=2500000007 (tx_hash=0xbe0ea7e2997b6554c4215076e80804a8894724c830ea35c2253ee3cbf1948265)
2022-07-11 21:05:04,878 INFO     Transaction StakedTokenAuctionHouse('0x328BCbF2d4c2D78936f52172c741456136FA7e92').restartAuction(13) was successful (tx_hash=0xbe0ea7e2997b6554c4215076e80804a8894724c830ea35c2253ee3cbf1948265)
```

\
