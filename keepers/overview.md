# Keepers Overview

Keepers are meant participate in collateral, surplus and debt auctions by directly interacting with GEB auction contracts deployed to the Ethereum blockchain.

### Responsibilities

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
* [Surplus](debt-auction-keeper/running-in-docker.md)
* [Debt](surplus-auction-keeper/running-in-docker.md)

### Running on a Host

Pre-requisites: Python 3.6+

```text
git clone https://github.com/makerdao/auction-keeper.git
cd auction-keeper
git checkout tags/prai-demo
git submodule update --init --recursive
pip3 install -r requirements.txt
```

Keeper can now be run with `bin/auction-keeper`

Examples:

* [Collateral](collateral-auction-keeper/running-on-a-host.md)
* [Surplus](debt-auction-keeper/running-on-a-host.md)
* [Debt](surplus-auction-keeper/running-on-a-host.md)

## Configuration Reference

Run `bin/auction-keeper -h` without arguments to see an up-to-date list of arguments and usage information.

### General

`--type collateral|surplus|debt` A keeper can only participate in one type of auction

`--collateral-type NAME` If `--type=collateral` is passed, the collateral\_type must also be provided. A keeper can only bid on a single collateral type. Note: Currently, only the `ETH-A` collateral type is used.

`--eth-from ADDRESS` Address of the keeper. Warnings: **Do not use the same `eth-from` account on multiple keepers** as it complicates `SAFEEngine` inventory management and will likely cause nonce conflicts. Using an `eth-from` account with an open SAFE is also discouraged.

`--rpc-host HOST` URI of ETH JSON-RPC node. Default `"http://localhost:8545"`

`--rpc-timeout SECS` Default `10`

This keeper connects to the Ethereum network using [Web3.py](https://github.com/ethereum/web3.py) and interacts with the GEB using [pyflex](https://github.com/reflexer-labs/pyflex). A connection to an Ethereum node \(`--rpc-host`\) is required. [Parity](https://www.parity.io/ethereum/) and [Geth](https://geth.ethereum.org/) nodes are supported over HTTP. Websocket endpoints are not supported by `pyflex`. A _full_ or _archive_ node is required; _light_ nodes are not supported.

If you don't wish to run your own Ethereum node, third-party providers are available. This software has been tested with [Infura](https://infura.io), [ChainSafe](https://chainsafe.io/) and [QuikNode](https://v2.quiknode.io/).

### Gas price strategies

The following options determine the keeper's gas strategy and are mutually exclusive:

`--ethgasstation-api-key MY_API_KEY` Use [ethgasstation.info](https://ethgasstation.info) for gas prices

`--etherchain-gas-price` Use [etherchain.org](https://etherchain.org) for gas prices

`--fixed-gas-price GWEI` Use a fixed gas price in GWEI

If none of these options is given or the gas API produces not result, the keeper will use gas price from your node.

### Other gas options

`--gas-initial-multiplier MULTIPLIER` When using an API source for initial gas price, tunes initial gas price. Ignored when using `--fixed-gas-price` or no strategy is given default `1.0`

`--gas-reactive-multiplier MULTIPLIER` Every 30 seconds, a transaction's gas price will be multiplied by this value until it is mined or `--gas-maxiumum` is reached. Not used if `gasPrice` is passed from your bidding model. Note: [Parity](https://wiki.parity.io/Transactions-Queue#dropping-conditions), as of this writing, requires a minimum gas increase of `1.125` to propagate transaction replacement; this should be treated as a minimum value unless you want replacements to happen less frequently than 30 seconds \(2+ blocks\). default `1.125`

`--gas-maximum GWEI` Maximum value for gas price

### Accounting options

By default the keeper `join`s system coins to `SAFEEngine` on startup and `exit`s system coin and collateral upon shutdown. The keeper provides facilities for managing `SAFEEngine` balances, which may be turned off to manage manually.

`--keep-system-coin-in-safe-engine-on-exit` Do not `exit` system coin on shutdown

`--keep-collateral-in-safe-engine-on-exit` Do not `exit` collateral on shutdown

`--return-collateral-interval SECS` Interval to `exit` won collateral to auction-keeper. Pass `0` to disable completely. default `300`

`--safe-engine-system-coin-target ALL|<integer>` Amount of system-coin the keeper will try to keep in the `SAFEEngine` through rebalancing with `join`s and `exit`s. By default, there is no target.

**Rebalancing:**

System coins are rebalanced per `--safe-engine-system-coin-target` when:

* The keeper starts up
* `SAFEEngine` balance is insufficient to place a bid
* An auction is settled

  Rebalances do not account for system coins moved from the `SAFEEngine` to an auction contract for an active bid.

  To avoid transaction spamming, small "dusty" system coins balances will be ignored \(until the keeper exits, if so configured\).

### Managing resources

#### Retrieving SAFEs

To start collateral auctions, the keeper needs a list of SAFEs and the collateralization ratio of each safe. There are two ways to retrieve the list of SAFEs:

`--from-block BLOCK_NUMBER` Scrape the chain for `ModifySAFECollateralization` events, starting at `BLOCK_NUMBER` Set this to the block where the first safe was created. After startup, only new blocks will be queried. NOTE: This can take significant time as the system matures. NOTE: To manage performance for debt auctions, periodically adjust `--from-block` to the block where the first liquidation which has not been `popDebtFromQueue`.

`--subgraph-endpoints NODE1,NODE2` Comma-delimited list of [Graph](https://thegraph.com) endpoints to retrieve `ModifySAFECollateralization` events. If multiple endpoints are specified, they will be tried in order if a communication failure occurs. NOTE: Currently only supported for collateral auctions Example with current Reflexer Graph endpoints: `--graph-endpoints https://api.thegraph.com/subgraphs/name/reflexer-labs/prai-mainnet,https://subgraph.reflexer.finance/subgraphs/name/reflexer-labs/prai`

#### Auctions

`--min-auction AUCTION_ID` Ignore auctions older than `AUCTION_ID`

`--max-auctions NUMBER` a Limit the number of bidding models created to handle active auctions.

`--block-check-interval <integer>, default:1` How often tocheck for new blocks

`--bid-check-interval <integer>, default 4` How often to check model process for new bid

Note: To use Infura free-tier and stay under the 100k requests/day quota, `--block-check-interval` must be greater than `10` and `--bid-check-interval` must be greater than 180. However, this will make your keeper slower in responding to auctions.

#### Sharding/Settling

Bid management can be sharded across multiple keepers by **auction id**. If you proceed with sharding, set these options:

`--shards NUMBER_OF_KEEPER` Number of keepers you will run. Set on all keepers

`--shard-id SHARD_ID` Set on each keeper, counting from 0.  
For example, to configure three keepers, set `--shards 3` and assign `--shard-id 0`, `--shard-id 1`, `--shard-id 2` for the three keepers.  
Note: **Auction starts are not sharded**. For an auction contract, only one keeper should be configured to `startAuction`.

If you are sharding across multiple accounts, you may wish to have another account handle all your `settleAuction`s.

`--settle-for <ACCOUNT1 ACCOUNT2>|NONE|ALL` Space-delimited list of accounts for which keeper will settle auctions or `NONE` to disable. If you'd like to donate your gas to settle auctions for all participants, `ALL` is also supported.  
Note: **Auction settlements are sharded**, so remove sharding configuration if running a dedicated auction settlement keeper.

#### Transaction management

`--bid-delay FLOAT`

Too many pending transactions can fill up the transaction queue, causing a subsequent transaction to be dropped. By waiting a small `--bid-delay` after each bid, multiple transactions can be submitted asynchronously while still allowing some time for older transactions to complete, freeing up the queue. Many parameters determine the appropriate amount of time to wait. For illustration purposes, assume the queue can hold 12 transactions, and gas prices are reasonable. In this environment, a bid delay of 1.2 seconds might provide ample time for transactions at the front of the queue to complete. [Etherscan.io](https://github.com/reflexer-labs/geb-docs/tree/ad25b15265b1f74d798690da41b1df00895f0cea/keepers/etherscan.io) can be used to view your account's pending transaction queue.

### Limitations

* If an auction started before the keeper was started, this keeper will not participate in it until the next block

  is mined.

* This keeper does not explicitly handle global settlement, and may submit transactions which fail during shutdown.
* Some keeper functions incur gas fees regardless of whether a bid is submitted.  This includes, but is not limited to,

  the following actions:

  * submitting approvals
  * adjusting the balance of surplus to debt
  * queuing debt for auction
  * liquidating a SAFE or starting a surplus or debt auction

* The keeper does not check model prices until an auction exists.  When configured to create new auctions, it will

  `liquidateSAFE`, start a new surplus or debt auction in response to opportunities regardless of whether or not your RAI or

  protocol token balance is sufficient to participate.  This too imposes a gas fee.

* Liquidating SAFEs to start new collateral auctions is an expensive operation.  To do so without a subgraph

  subscription, the keeper initializes a cache of safe state by scraping event logs from the chain.  The keeper will then

  continuously refresh safe state to detect undercollateralized SAFEs.

  * Despite batching log queries into multiple requests, Geth nodes are generally unable to initialize the safe state

    cache in a reasonable amount of time.  As such, Geth is not recommended for liquidating SAFEs.

  * To manage resources, it is recommended to run separate keepers using separate accounts to liquidate \(`--start-auctions-only`\)

    and bid \(`--bid-only`\).

For some known Ubuntu and macOS issues see the [pyflex](https://github.com/reflexer-labs/pyflex) README.

### Testing

This project uses [pytest](https://docs.pytest.org/en/latest/) for unit testing. Testing depends upon a dockerized local testchain included in `lib\pyflex\tests\config`.

In order to be able to run tests:

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

### Support

[https://discord.gg/kB4vcYs](https://discord.gg/kB4vcYs)

### License

See [COPYING](https://github.com/makerdao/auction-keeper/blob/master/COPYING) file.

#### Disclaimer

YOU \(MEANING ANY INDIVIDUAL OR ENTITY ACCESSING, USING OR BOTH THE SOFTWARE INCLUDED IN THIS GITHUB REPOSITORY\) EXPRESSLY UNDERSTAND AND AGREE THAT YOUR USE OF THE SOFTWARE IS AT YOUR SOLE RISK. THE SOFTWARE IN THIS GITHUB REPOSITORY IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE. YOU RELEASE AUTHORS OR COPYRIGHT HOLDERS FROM ALL LIABILITY FOR YOU HAVING ACQUIRED OR NOT ACQUIRED CONTENT IN THIS GITHUB REPOSITORY. THE AUTHORS OR COPYRIGHT HOLDERS MAKE NO REPRESENTATIONS CONCERNING ANY CONTENT CONTAINED IN OR ACCESSED THROUGH THE SERVICE, AND THE AUTHORS OR COPYRIGHT HOLDERS WILL NOT BE RESPONSIBLE OR LIABLE FOR THE ACCURACY, COPYRIGHT COMPLIANCE, LEGALITY OR DECENCY OF MATERIAL CONTAINED IN OR ACCESSED THROUGH THIS GITHUB REPOSITORY.

