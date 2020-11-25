---
description: Running a collateral auction-keeper
---

# Collateral Auction Keeper

## Quickstart

1\) Buy RAI from [Uniswap v2](https://info.uniswap.org/pair/0xEBdE9F61e34B7aC5aAE5A4170E964eA85988008C) or [open a SAFE](https://app.gitbook.com/@reflexer-labs/s/geb/pyflex/safe-management/opening-a-safe) and generate some RAI.

2\) Create  `run_auction_keeper.sh`

```text
#!/bin/bash

docker run -it \
	-v <KEYSTORE DIR>:/keystore \
	reflexer/auction-keeper \
        --rpc-uri <ETH_RPC_URL> \
        --eth-from <KEEPER ADDRESS> \
        --eth-key "key_file=/keystore/<KEYSTORE FILE>"
        
```

#### Substitute in the following variables:

`KEYSTORE_DIR` The local directory where your keystore file is.

`ETH_RPC_URL`URL of ethereum RPC connection

`KEEPER_ADDRESS`Address of your keeper. Should be checksummed format, not in lowercase.

3\). `chmod +x run_auction_keeper.sh`

4\) `./run_auction_keeper.sh`

```text
$ ./run_auction_keeper.sh
Using default tag: latest
latest: Pulling from reflexer/auction-keeper
Digest: sha256:5d75a47028a0867b618b568269d985ac4a68ea8c63a920ac18093db3c3064134
Status: Image is up to date for reflexer/auction-keeper:latest
docker.io/reflexer/auction-keeper:latest
Password for /keystore/key3.json: 
```

5\) Enter your keystore file password. 

#### Keeper output

```text
Keeper connected to RPC connection http://172.31.46.181:8545
Keeper operating as 0x02b70C78b400FF8fe89Af7D84d443f875D047a8F
Executing keeper startup logic
Checking if internal system coin balance needs to be rebalanced
Joining 86.380323152450242963 system coin to the SAFE Engine
Sent transaction <pyflex.gf.CoinJoin object at 0x7ff7d69affd0>.join('0x02b70C78b400FF8fe89Af7D84d443f875D047a8F', 86380323152450242963) with nonce=127, gas=174888, gas_price=33000000000 (tx_hash=0x885216086bdd41a5bf3be858d77161fac3433ffd3a208571f2cf52ed51456d83)
Transaction <pyflex.gf.CoinJoin object at 0x7ff7d69affd0>.join('0x02b70C78b400FF8fe89Af7D84d443f875D047a8F', 86380323152450242963) was successful (tx_hash=0x885216086bdd41a5bf3be858d77161fac3433ffd3a208571f2cf52ed51456d83)
```

```text
Keeper will perform the following operation(s) in parallel:
--> Check all safes and start new auctions if any critical safes need to be liquidated
--> Check all auctions being monitored and evaluate bidding opportunity every 4.0 seconds
--> Check all auctions and settle for 0x02b70C78b400FF8fe89Af7D84d443f875D047a8F
*** When Keeper is settling/bidding, the initial evaluation of auctions will likely take > 45 minutes without setting a lower boundary via '--min-auction' ***
*** When Keeper is starting auctions, initializing safe history may take > 30 minutes without using Graph via `--graph-endpoints` ***
Keeper will use Node gas price (currently 33.0 Gwei, changes over time) and will multiply by 1.125 every 30s to a maximum of 2000.0 Gwei for transactions and bids unless model instructs otherwise
Watching for new blocks
Started 2 timer(s)
Checked 39 safes in 8 seconds
```

```text
2020-11-25 19:57:10,230 INFO     Checked auctions 0 to 4 in 0 seconds
2020-11-25 19:57:10,805 INFO     Checked 39 safes in 0 seconds
2020-11-25 19:57:10,952 INFO     Checked auctions 0 to 4 in 0 seconds
2020-11-25 19:57:14,847 INFO     Checked 39 safes in 0 seconds
2020-11-25 19:57:15,009 INFO     Checked auctions 0 to 4 in 0 seconds
2020-11-25 19:57:51,377 INFO     Checked 39 safes in 0 seconds
2020-11-25 19:57:51,733 INFO     Checked auctions 0 to 4 in 0 seconds
2020-11-25 19:57:55,379 INFO     Checked 39 safes in 0 seconds
2020-11-25 19:57:55,483 INFO     Checked auctions 0 to 4 in 0 seconds
2020-11-25 19:58:21,662 INFO     Checked 39 safes in 0 seconds

```

