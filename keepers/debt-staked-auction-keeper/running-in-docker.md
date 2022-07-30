---
description: Running a debt staked auction keeper in a docker container
---

# Running in Docker

### 1. Get RAI

Buy RAI or [open a SAFE](https://app.gitbook.com/@reflexer-labs/s/geb/pyflex/safe-management/opening-a-safe) to generate it.

### 2. Modify the model file as needed

A basic debt staked auction bidding model can be found in `models/debt_staked_model.py`. This model retrieves the latest FLX/USD price from Coingecko and will automatically place bids in an auction.

You probably want to modify the following variables in `models/debt_model.py`:

* `MAXIMUM_FLX_MULTIPLIER`: the maximum acceptable FLX price to use when bidding. Default: `0.90` meaning the maximum price to pay when biding for FLX (with RAI) is 90% of the current FLX/USD market price from Coingecko
* `MINIMUM_FLX_MULTIPLIER`: the minimumum FLX price to use when bidding. This will determine your opening bid.Default: `0.50` meaning the minimumm price to pay when biding for FLX (with RAI) is 50% of the current FLX/USD market price from Coingecko
* `MY_BID_INCREASE`: the bid increase (in RAI) to propose when outbidding another bidder. If the value is smaller than the debt staked auction house's `bidIncrease`, then it will use the value set in the debt staked auction house. Example: a value of `1.10` will use bid increases of 10%. Note: the current `bidIncrease` on mainnet is `1.05`

Then, use `chmod +x debt_staked_model.py`.

For more information about bidding models, see [this](https://docs.reflexer.finance/keepers/bidding-models).

### 3) Modify the keeper run file

Modify the following variables in `run_debt_keeper.sh`:

* `KEEPER_ADDRESS` - the keeper's address. It should be in checksummed format (not lowercase)
* `ETH_RPC_URL` - the URL of your Ethereum RPC connection
* `KEYSTORE_DIR` - the full path of the directory where your keystore file is
* `MODEL_DIR` - the full path of directory where your `surplus_model.py` file is
* `KEYSTORE_FILE` - your Ethereum UTC JSON keystore filename. For more information about the keystore format and how to generate it, check [Ethereum UTC / JSON Wallet Encryption](https://wizardforcel.gitbooks.io/practical-cryptography-for-developers-book/content/symmetric-key-ciphers/ethereum-wallet-encryption.html) or[ keythereum](https://github.com/ethereumjs/keythereum).
* `GAS_MAXIMUM` -maximum gas price, in GWEI

Then, use `chmod +x run_debt_keeper.sh`.

### 4) Start the keeper and enter your keystore file password

Use `./run_debt_staked_keeper.sh`.

```
$ ./run_debt_staked_keeper.sh
latest: Pulling from reflexer/auction-keeper
Digest: sha256:7e55ec9b0a136fc903d9f7f2690538bcbde9029d957e0e6f84d0282790f9666a
Status: Downloaded newer image for reflexer/auction-keeper:latest
docker.io/reflexer/auction-keeper:latest
Password for /keystore/key.json:
```

