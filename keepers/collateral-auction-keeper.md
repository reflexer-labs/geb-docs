---
description: Running a collateral auction-keeper
---

# Collateral Auction Keeper

## Quickstart

1\) Buy RAI from [Uniswap v2](https://info.uniswap.org/pair/0xEBdE9F61e34B7aC5aAE5A4170E964eA85988008C) or open a SAFE and generate some RAI.

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

4\) `./run_auction`\_`keeper.sh`

