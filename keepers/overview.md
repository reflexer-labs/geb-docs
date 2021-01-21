# Overview

Keepers are meant participate in collateral, surplus and debt auctions by directly interacting with GEB auction contracts deployed to the Ethereum blockchain.

### Keeper Responsibilities

The keepers are responsible with:

1. Monitoring all active auctions
2. Starting new auctions 
3. Discovering new auctions 
4. Ensuring a bidding model is running for each active auction 
5. Passing auction status to each bidding model 
6. Processing each bidding model output and submitting bids

### Architecture

`auction-keeper` can read an auction's status directly from the Ethereum blockchain or from a [Graph](https://thegraph.com/) node. Its unique feature is the ability to plug in external _bidding models_ which tell the keeper when and how much to bid. Bid prices are received from separate _bidding models_.

_Bidding models_ are simple processes that can be implemented in any programming language. They only need to pass JSON objects to and from `auction-keeper`. The simplest example of a bidding model is a shell script which echoes a fixed price.

For every new block, all auctions from `1` to `auctionsStarted` are checked for active status. If a new auction is detected, a new bidding model is started.

**NOTE**: _Bidding models_ are only used for surplus and debt auctions, not collateral auctions.

## Installation

### Running on Docker \(recommended\)

Examples:

* [Collateral](collateral-auction-keeper/running-in-docker.md)
* [Surplus](surplus-auction-keeper/running-in-docker.md)
* [Debt](debt-auction-keeper/running-in-docker.md)

### Running on a host

Pre-requisites: Python 3.6+

Install `auction-keeper` dependencies with:

```text
git clone https://github.com/reflexer-finance/auction-keeper.git
cd auction-keeper
git checkout tags/prai-demo
git submodule update --init --recursive
pip3 install -r requirements.txt
```

The keeper can now be run with `bin/auction-keeper`.

Auction specific examples:

* [Collateral](collateral-auction-keeper/running-on-a-host.md)
* [Surplus](surplus-auction-keeper/running-on-a-host.md)
* [Debt](debt-auction-keeper/running-on-a-host.md)

## Configuration Reference

Run `bin/auction-keeper -h` to see an up-to-date list of arguments and usage information.

### General

`--type collateral|surplus|debt` A keeper can only participate in one type of auction

`--collateral-type NAME` If `--type=collateral` is passed, the `collateral_type` must also be provided. A keeper can only bid on a single collateral type auction at a time. **NOTE**: Currently, only the `ETH-A` collateral type is used.

`--eth-from ADDRESS` Address of the keeper. **Warning**: **Do not use the same `eth-from` account on multiple keepers** as it complicates `SAFEEngine` inventory management and will likely cause nonce conflicts. Using an `eth-from` account with an open SAFE is also discouraged.

`--rpc-host HOST` URI of ETH JSON-RPC node. Default `"http://localhost:8545"`

`--rpc-timeout SECS` Defaults to `10`

The keeper connects to the Ethereum network using [Web3.py](https://github.com/ethereum/web3.py) and interacts with GEB using [pyflex](https://github.com/reflexer-labs/pyflex). A connection to an Ethereum node \(`--rpc-host`\) is required. [Parity](https://www.parity.io/ethereum/) and [Geth](https://geth.ethereum.org/) nodes are supported over HTTP. Websocket endpoints are not supported in `pyflex`. A _full_ or _archive_ node is required; _light_ nodes are **not** supported.

If you don't want to run your own Ethereum node, third-party providers are available. This software has been tested with [Infura](https://infura.io), [ChainSafe](https://chainsafe.io/) and [QuikNode](https://v2.quiknode.io/).

### Gas price strategies

The following options determine the keeper's gas strategy and are mutually exclusive:

`--ethgasstation-api-key MY_API_KEY` Use [ethgasstation.info](https://ethgasstation.info) for gas prices

`--etherchain-gas-price` Use [etherchain.org](https://etherchain.org) for gas prices

`--poanetwork-gas-price` Use [poa.network](https://gasprice.poa.network/) for gas prices

`--etherscan-gas-price` Use [etherscan.io](https://etherscan.io/) for gas prices. Optional: `--etherscan-key KEY`. Rate-limited to 1request/5sec w/o a key.

`--gasnow-gas-price` Use [gasnow.org](https://gasnow.org) for gas prices

`--fixed-gas-price GWEI` Use a fixed gas price \(in GWEI\)

If none of these options is given or if the gas API produces no result, the keeper will fetch the gas price from the node you connected to.

### Other gas options

`--gas-initial-multiplier MULTIPLIER` When using an API source for fetching the initial gas price, this tunes the price. It's ignored when you're using `--fixed-gas-price`. In case no strategy is specified it defaults to `1.0`

`--gas-reactive-multiplier MULTIPLIER` Every 30 seconds, a transaction's gas price will be multiplied by this value until it is mined or `--gas-maxiumum` is reached. Not used if `gasPrice` is passed from your bidding model. **NOTE**: [Parity](https://wiki.parity.io/Transactions-Queue#dropping-conditions), as of this writing, requires a minimum gas increase of `1.125` to propagate a transaction replacement; this should be treated as a minimum value unless you want replacements to happen less frequently. This multiplier defaults to `1.125` if no other value is given.

`--gas-maximum GWEI` Maximum value for gas price

### Accounting options

By default the keeper `join`s system coins to `SAFEEngine` on startup and `exit`s all system coins and collateral upon shutdown. The keeper provides options for managing `SAFEEngine` balances, which may be turned off in case you'd like to manage balances manually.

`--keep-system-coin-in-safe-engine-on-exit` Do not `exit` system coin on shutdown

`--keep-collateral-in-safe-engine-on-exit` Do not `exit` collateral on shutdown

`--return-collateral-interval SECS` How often, in seconds, the keeper `exit`s won collateral from `SAFEEngine`. Pass `0` to disable completely. Defaults to `300`

`--safe-engine-system-coin-target ALL|<integer>` Amount of system coins the keeper will try to keep in `SAFEEngine` by rebalancing with `join`s and `exit`s between its own wallet and its balance inside GEB. Defaults to `ALL` and the keeper will `join` all of an account's systems coins.

### **Rebalancing**

System coins are rebalanced per `--safe-engine-system-coin-target` when:

* The keeper starts up
* `SAFEEngine` balance is insufficient in order to place a bid
* An auction is settled

Rebalances do not account for system coins moved from the `SAFEEngine` to an auction contract for an active bid.

To avoid transaction spamming, small "dusty" system coins balances will be ignored \(until the keeper exits, if so configured\).

### Managing resources

#### Retrieving SAFEs

To start collateral auctions, the keeper needs a list of SAFEs and the collateralization ratio of each safe. There are two ways to retrieve the list of open SAFEs:

`--from-block BLOCK_NUMBER` Scrape the chain for `ModifySAFECollateralization` events, starting at `BLOCK_NUMBER` . Set this to the block where the first ever SAFE was created. After startup, only new blocks will be queried. The scrape process can last a significant amount of time as the system matures. **NOTE**: To manage the performance of debt auction bidding, periodically adjust `--from-block` to the block number of the oldest liquidation which has not been `popDebtFromQueue`d yet. Defaults to `geb.starting_block_number`, the block in which the system was deployed.

`--graph-endpoints NODE1,NODE2` Comma delimited list of [Graph](https://thegraph.com) endpoints used to retrieve `ModifySAFECollateralization` events. If multiple endpoints are passed, they will be pinged sequentially in the order they were specified in case one or many of them fail. **NOTE**: This flag is only supported for collateral auctions.

`--graph-block-threshold NUMBER_OF_BLOCKS` When the keeper fetches SAFE data to find critical safes, use the `--graph-endpoints` when the keeper's last processed block is older than `NUMBER_OF_BLOCKS`. The graph will be faster than a node when fetching historical data, but recent graph blocks might be slightly delayed compared to an ethereum node. This allows the keeper to to fetch historical data from the graph, but use the node for all newer blocks. Defaults to `20`

The following are the most recent Graph node endpoints for RAI:`--graph-endpoints https://subgraph.reflexer.finance/subgraphs/name/reflexer-labs/prai,https://api.thegraph.com/subgraphs/name/reflexer-labs/prai-mainnet`

#### Auctions

`--min-auction AUCTION_ID` Ignore auctions older than `AUCTION_ID`

`--max-auctions NUMBER` Limit the number of bidding models created to handle active auctions.

`--block-check-interval <integer>, default:1` How often the keeper checks for new blocks

`--bid-check-interval <integer>, default 4` How often the keeper checks model processes for new bids

**NOTE**: if you'd like to use Infura with your keeper and prefer the free-tier \(you do less than 100K requests per day\), `--block-check-interval` must be greater than `10` and `--bid-check-interval` must be greater than 180. However, this will make your keeper slower and it will not quickly bid in auctions.

#### Flash swaps

Flash swaps allow a keeper to participate in collateral auctions without any system coins. The flash swap borrows the system coin necessary for the collateral auctions, wins the collateral at a discount and transferred the won collateral back to the keeper, all in one transaction. Note: If the overall transaction is not profitable, the swap will fail. The keeper only needs enough ether to pay for the gas of the swap.

`--flash-swap` Turn on Uniswap flash swaps for collateral auctions. Not supported for `--type debt` or `--type surplus`

[Read more about flash swaps](collateral-auction-keeper/flash-swaps.md)

#### Sharding/Settling

Bid management can be sharded across multiple keepers. If you want to proceed with sharding, set these options:

`--shards NUMBER_OF_KEEPER` Number of keepers you plan to run. You must set this for all keepers

`--shard-id SHARD_ID` You must specify this for every keeper, counting from 0

For example, to configure three keepers, set `--shards 3` and assign `--shard-id 0`, `--shard-id 1`, `--shard-id 2` for the first, second and third keeper.

**NOTE**: **Auction starts are not sharded**. Only one keeper should be configured to `liquidateSAFE`s and this way `startAuction`s.

If you are sharding across multiple accounts, you may want to have a separate keeper that handles all your `settleAuction`s \(in the case of English collateral, debt and surplus auctions\)

`--settle-for <ACCOUNT1 ACCOUNT2>|NONE|ALL` Space-delimited list of accounts for which the keeper will settle auctions. Specify `NONE` to disable this option. If you'd like to donate your gas to settle auctions for all participants, `ALL` is also supported. Defaults to only the keeper address.

**NOTE**: **Auction settlements are already sharded**, so you should remove the sharding configuration if you're running a dedicated auction settlement keeper.

#### Transaction management

`--bid-delay FLOAT`

Many pending transactions can fill up the keeper's transaction queue, causing every subsequent transaction to be dropped. By waiting a small `--bid-delay` after each bid, multiple transactions can be submitted asynchronously while still allowing some time for older transactions to complete, freeing up the queue.

Many parameters determine the appropriate bid delay. For illustration purposes, assume the queue can hold 12 transactions, and gas prices are reasonable. In this setup, a bid delay of 1.2 seconds might provide ample time for transactions at the front of the queue to complete.

### Limitations

* If an auction started before the keeper was started, this keeper will not participate in it until the next block is mined
* This keeper does not explicitly handle Global Settlement, and may submit transactions which fail during shutdown
* Some keeper functions incur gas fees regardless of whether a bid is submitted.  This includes, but is not limited to, the following actions:
  * submitting token approvals
  * adjusting the surplus and debt balances from `AccountingEngine`
  * liquidating a SAFE or starting a surplus or debt auction
* The keeper does not check model prices until an auction exists.  When configured to create new auctions, it will `liquidateSAFE`s or start a new surplus or debt auction regardless of whether or not your system coin or protocol token balance is sufficient to bid
* Liquidating a SAFE to start a new collateral auction is a time consuming operation. To do so without a subgraph subscription, the keeper initializes a cache of SAFEs states by scraping event logs from the chain. The keeper will then continuously refresh SAFE states and detect undercollateralized SAFEs.
  * Despite batching log queries into multiple requests, Geth nodes are generally unable to initialize the SAFE state cache in a reasonable amount of time.  As such, **Geth is not recommended for liquidating SAFEs**
  * To manage resources, it is recommended to run separate keepers using separate accounts to liquidate \(`--start-auctions-only`\) and bid \(`--bid-only`\) in auctions

For some known Ubuntu and macOS issues see the [pyflex](https://github.com/reflexer-labs/pyflex) README.

### Testing

This project uses [pytest](https://docs.pytest.org/en/latest/) for unit testing. Testing depends on a dockerized local testchain included in `lib\pyflex\tests\config`.

In order to be able to run tests you should execute:

```text
git clone https://github.com/reflexer-labs/auction-keeper.git
cd auction-keeper
git checkout tags/prai-demo
git submodule update --init --recursive
./install.sh
source _virtualenv/bin/activate
pip3 install -r requirements-dev.txt
```

You can then run all tests with:

```text
./test.sh
```

