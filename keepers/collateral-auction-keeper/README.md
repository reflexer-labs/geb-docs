---
description: Setup and the life cycle of a collateral auction keeper
---

# Collateral Auction Keeper

## Quickstart

Running a collateral auction keeper takes 5 steps. 

### 1\) Get RAI 

Buy RAI from [Uniswap v2](https://info.uniswap.org/pair/0xEBdE9F61e34B7aC5aAE5A4170E964eA85988008C) or [open a SAFE](https://app.gitbook.com/@reflexer-labs/s/geb/pyflex/safe-management/opening-a-safe) and generate it.

### 2\) Create the keeper run file

Create a file called  `run_auction_keeper.sh` and paste the following code in it:

```text
#!/bin/bash

docker run -it \
	-v <KEYSTORE DIR>:/keystore \
	reflexer/auction-keeper \
        --rpc-uri <ETH_RPC_URL> \
        --eth-from <KEEPER ADDRESS> \
        --eth-key "key_file=/keystore/<KEYSTORE FILE>"
        
```

#### Then, substitute the following variables:

`KEYSTORE_DIR` - this must be the local directory where your keystore file is.

`ETH_RPC_URL` - this is the URL of ethereum RPC connection

`KEEPER_ADDRESS` - this is your keeper's address. It should be in checksummed format, not lowercase

### 3\) Make the keeper script runnable

`chmod +x run_auction_keeper.sh`

### 4\) Run the keeper

`./run_auction_keeper.sh`

```text
$ ./run_auction_keeper.sh
Using default tag: latest
latest: Pulling from reflexer/auction-keeper
Digest: sha256:5d75a47028a0867b618b568269d985ac4a68ea8c63a920ac18093db3c3064134
Status: Image is up to date for reflexer/auction-keeper:latest
docker.io/reflexer/auction-keeper:latest
Password for /keystore/key.json: 
```

### 5\) Enter your keystore file password

## System Coin Join

```text
Keeper connected to RPC connection http://172.31.46.181:8545
Keeper operating as 0x02b70C78b400FF8fe89Af7D84d443f875D047a8F
Executing keeper startup logic
Checking if internal system coin balance needs to be rebalanced
Joining 86.380323152450242963 system coin to the SAFE Engine
Sent transaction <pyflex.gf.CoinJoin object at 0x7ff7d69affd0>.join('0x02b70C78b400FF8fe89Af7D84d443f875D047a8F', 86380323152450242963) with nonce=127, gas=174888, gas_price=33000000000 (tx_hash=0x885216086bdd41a5bf3be858d77161fac3433ffd3a208571f2cf52ed51456d83)
Transaction <pyflex.gf.CoinJoin object at 0x7ff7d69affd0>.join('0x02b70C78b400FF8fe89Af7D84d443f875D047a8F', 86380323152450242963) was successful (tx_hash=0x885216086bdd41a5bf3be858d77161fac3433ffd3a208571f2cf52ed51456d83)
```

By default, the keeper will `join` all of its system coins to the [SAFEEngine](https://docs.reflexer.finance/system-contracts/core/safe-engine) in order to make them available for bidding in collateral auctions. If you want to change the amount used for purchases, you can add the following flag to `run_auction_keeper.sh`

`--safe-engine-system-coin-target <Number of system coins>, default: ALL`

## Rebalancing

The keeper will periodically rebalance between ETH bought from collateral auctions and RAI to ensure `--safe-engine-system-coin-targetsystem` coins are available in the [SAFEEngine](https://docs.reflexer.finance/system-contracts/core/safe-engine). 

If the keeper's SAFEEngine balance drops below `--safe-engine-system-coin-targetsystem` \(due to bidding\), the keeper will automatically try to join additional system coins by swapping ETH for RAI.

## Setting the Gas Price

By default, the keeper uses the node's gas price when creating transactions.  Every 30 seconds, the keeper will multiple the gas price by `--gas-reactive-multiplier, default: 1.125`  up to `--gas-maximum, default: 2000`

#### Gas Provider

If you don't want to use the node's gas prices, you can use one of these providers instead:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Provider</th>
      <th style="text-align:left">Flags</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><a href="https://ethgasstation.info">ethgasstation.info</a>
      </td>
      <td style="text-align:left"><code>--ethgasstation-api-key &lt;API_KEY&gt;</code> 
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://etherscan.io">etherscan.io</a>
      </td>
      <td style="text-align:left">
        <p><code>--etherscan-gas-price</code>,</p>
        <p>Queries are rate-limited without an API Key</p>
        <p><code>--etherscan-key &lt;API_KEY&gt;</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="htps://gasnow.org">gasnow.org</a>
      </td>
      <td style="text-align:left"><code>--gasnow-gas-price</code>, optional:<code>--gasnow-app-name &lt;APP_NAME&gt;</code>
      </td>
    </tr>
  </tbody>
</table>

You can further adjust the gas price with:

* `--fixed-gas-price <GWEI>` - sets a fixed gas price to use for every keeper interaction with the chain
* `--gas-initial-multiplier` \(default: 1.0\) - sets the initial gas multiplier

## Fetching SAFEs Histories

During startup, the keeper will fetch all SAFE's histories:

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

### Using The Graph to Fetch SAFE Data

[The Graph](https://thegraph.com/) is a project that seeks to provide efficient and simplified access to Ethereum data. Our keeper implementation allows anyone to connect to RAI's subgraph and pull data from the mainnet GEB instance using a developer friendly API interface.

Instead of using the node to gather all past SAFE history, you can add the following flag to `run_auction_keeper.sh`:

```text
--graph-endpoints https://api.thegraph.com/subgraphs/name/reflexer-labs/prai-mainnet,https://subgraph.reflexer.finance/subgraphs/name/reflexer-labs/rai
```

Notice that we're adding two different endpoints so that the keeper has a fallback in case one of them returns faulty results.

Adding `--graph-endpoints` will speed up the process of fetching the initial history for all SAFEs when the keeper starts up. Notice that the SAFE history is fetched in only 3 seconds with `--graph-endpoints` and it takes longer without it. 

As more and more SAFEs are created, getting their histories without `--graph-endpoints`will take longer.

```text
Watching for new blocks
Started 2 timer(s)
Getting safe mods from https://api.thegraph.com/subgraphs/name/reflexer-labs/prai-mainnet
Fetching safe modes from https://api.thegraph.com/subgraphs/name/reflexer-labs/prai-mainnet
Checked 39 safes in 3 seconds
```

## Stopping the Keeper

```text
Keeper received SIGINT/SIGTERM signal, will terminate gracefully
The keeper is terminating due do SIGINT/SIGTERM signal received
Shutting down the keeper
Waiting for all threads to terminate...
Waiting for outstanding callback to terminate...
Waiting for outstanding timers to terminate...
Executing keeper shutdown logic...
Exiting 86.380323152450242963 system coin from the SAFE Engine before shutdown
Sent transaction <pyflex.gf.CoinJoin object at 0x7ff7d69affd0>.exit('0x02b70C78b400FF8fe89Af7D84d443f875D047a8F', 86380323152450242963) with nonce=128, gas=174631, gas_price=33580460345 (tx_hash=0x96984500294ec64212990e3b9131e8442d17b8f1d9e9ba5ef61d5d61d79d18e9)
Transaction <pyflex.gf.CoinJoin object at 0x7ff7d69affd0>.exit('0x02b70C78b400FF8fe89Af7D84d443f875D047a8F', 86380323152450242963) was successful (tx_hash=0x96984500294ec64212990e3b9131e8442d17b8f1d9e9ba5ef61d5d61d79d18e9)
Shutdown logic finished
Keeper terminated
```

Notice that the keeper automatically `exit`s all system coins from the `SAFEEngine` when it shuts down.

If you want to keep system coins in the `SAFEEngine` on shutdown, you can add the  `--keep-system-coin-in-safe-engine-on-exit` flag to `run_auction_keeper.sh`This will skip the `exit` operation on shutdown and the `join` on the next startup, thus saving gas.

