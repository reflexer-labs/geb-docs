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

## System coin join

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

{% hint style="warning" %}
The keeper will periodically rebalance to ensure `--safe-engine-system-coin-target`system coin is available in the SAFEEngine for collateral auctions. If the keeper's SAFEEngine balance drops below this target\(due to purchasing discounted collateral\), the keeper will automatically try to `join` additional system coins from the keeper's address if available.
{% endhint %}

#### Gas price

```text
Keeper will perform the following operation(s) in parallel:
--> Check all safes and start new auctions if any critical safes need to be liquidated
--> Check all auctions being monitored and evaluate bidding opportunity every 4.0 seconds
--> Check all auctions and settle for 0x02b70C78b400FF8fe89Af7D84d443f875D047a8F
*** When Keeper is settling/bidding, the initial evaluation of auctions will likely take > 45 minutes without setting a lower boundary via '--min-auction' ***
*** When Keeper is starting auctions, initializing safe history may take > 30 minutes without using Graph via `--graph-endpoints` ***
Keeper will use Node gas price (currently 33.0 Gwei, changes over time) and will multiply by 1.125 every 30s to a maximum of 2000.0 Gwei for transactions and bids unless model instructs otherwise
```

By default, the keeper uses the node's gas price when creating transactions.  Every 30 seconds, the keeper will multiple the gas price by `--gas-reactive-multiplier, default 1.125`  up to `--gas-maximum, default:2000`

#### Gas Provider

If you don't want to use the node's gas prices, you can use one of these providers for gas prices.

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
      <td style="text-align:left"><a href="https://www.poa.network">poa.network</a>
      </td>
      <td style="text-align:left"><code>--poanetwork-gas-price</code> , optional: <code>--poanetwork-url</code> &lt;URL&gt;</td>
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
    <tr>
      <td style="text-align:left">Fixed gas price</td>
      <td style="text-align:left"><code>--fixed-gas-price &lt;GWEI&gt;</code>
      </td>
    </tr>
  </tbody>
</table>

You can further adjust a provider's gas price with `--gas-initial-multiplier, default: 1.0`

####  Fetching SAFE histories

During startup, the keeper needs to fetch SAFE histories.

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

#### Checking auctions and new SAFE events

```text
hecked auctions 0 to 4 in 0 seconds
Checked 39 safes in 0 seconds
Checked auctions 0 to 4 in 0 seconds
Checked 39 safes in 0 seconds
Checked auctions 0 to 4 in 0 seconds
Checked 39 safes in 0 seconds
Checked auctions 0 to 4 in 0 seconds
Checked 39 safes in 0 seconds
Checked auctions 0 to 4 in 0 seconds
Checked 39 safes in 0 seconds

```

#### Fetching SAFE histories: The Graph

The[ graph](https://thegraph.com) is a project that seeks to provide efficient and simplified access to Ethereum data.

Instead of using the node to gather all past SAFE history, you can add the following flag to `run_auction`\_`keeper.sh`

`--graph-endpoints https://api.thegraph.com/subgraphs/name/reflexer-labs/prai-mainnet,https://subgraph.reflexer.finance/subgraphs/name/reflexer-labs/rai`

This will speed up fetching the initial history of all SAFEs when the collateral keeper starts up. Notice the SAFE history is fetched in only 3 seconds with `--graph-endpoits`,while it took 8 seconds above. This is negligible now, but as more SAFEs are created, getting SAFE history without `--graph-endpoints`will take longer and longer.

```text
Watching for new blocks
Started 2 timer(s)
Getting safe mods from https://api.thegraph.com/subgraphs/name/reflexer-labs/prai-mainnet
Fetching safe modes from https://api.thegraph.com/subgraphs/name/reflexer-labs/prai-mainnet
Checked 39 safes in 3 seconds
```

#### Exiting collateral-keeper

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

Notice, the keep automatically `exits` $$a = b$$ system coin from the system when it shuts down.

If you want to keep system coin in the SAFEEngine on shutdown, you can add the  `--keep-system-coin-in-safe-engine-on-exit` flag to `run_auction`\_`keeper.sh`This will skip the `exit` on shutdown and the `join` on the next startup, saving gas.

