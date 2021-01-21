---
description: Running a debt auction keeper in a Docker container
---

# Running in Docker

_**Not available for PRAI**_

## 1\) Get RAI

Buy RAI from Uniswap v2 or [open a SAFE](https://app.gitbook.com/@reflexer-labs/s/geb/pyflex/safe-management/opening-a-safe) and generate it.

## 2\) Create a model file

Pick a system coin/protocol token price you're willing to bid and paste the following code into `debt_model.sh`:

```text
#!/usr/bin/env bash
while true; do
  echo "{\"price\": \"325.0\"}"
  sleep 120                   
done
```

### Then:

`chmod +x debt_model.sh`

For more information about bidding models, see [this](https://docs.reflexer.finance/keepers/bidding-models).

## 3\) Create the keeper run file

Create a file called `run_auction_keeper.sh` and paste the following code in it:

```text
#!/bin/bash

docker run -it \
  -v <KEYSTORE_DIR>:/keystore \
  -v <MODEL_DIR>:/models \
    reflexer/auction-keeper:prai-demo \
        --type debt \
        --model /models/debt_model.sh \
        --rpc-uri <ETH_RPC_URL> \
        --eth-from <KEEPER_ADDRESS> \
        --eth-key key_file=/keystore/<KEYSTORE_FILE>
```

### Then, substitute the following variables:

`KEYSTORE_DIR` - the local directory where your keystore file is

`MODEL_DIR` - the local directory where your `debt_model.sh` file is

`KEYSTORE_FILE` - your Ethereum UTC JSON keystore filename

For more information about this keystore format and how to generate them use:

* [Ethereum UTC / JSON Wallet Encryption](https://wizardforcel.gitbooks.io/practical-cryptography-for-developers-book/content/symmetric-key-ciphers/ethereum-wallet-encryption.html)
* [keythereum](https://github.com/ethereumjs/keythereum)

`ETH_RPC_URL` - the URL of your ethereum RPC connection

`KEEPER_ADDRESS` - the keeper's address. It should be in checksummed format \(not lowercase\).

### Then:

`chmod +x run_auction_keeper.sh`

## 4\) Start the keeper and enter your keystore file password

`./run_auction_keeper.sh`

```text
$ ./run_auction_keeper.sh
prai-demo: Pulling from reflexer/auction-keeper
Digest: sha256:7e55ec9b0a136fc903d9f7f2690538bcbde9029d957e0e6f84d0282790f9666a
Status: Downloaded newer image for reflexer/auction-keeper:prai-demo
docker.io/reflexer/auction-keeper:prai-demo
Password for /keystore/key.json:
```

## Debt Auctioning Process

[Debt Auctioning Process](debt-auctions.md)
