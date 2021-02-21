---
description: Running a debt auction keeper directly on a host
---

# Running on a Host


## Prerequisties

Python 3.6+

### Get RAI

Buy RAI from Uniswap v2 or [open a SAFE](https://app.gitbook.com/@reflexer-labs/s/geb/pyflex/safe-management/opening-a-safe) and generate it.

### Clone

```text
git clone https://github.com/reflexer-labs/auction-keeper.git
cd auction-keeper
git submodule update --init --recursive
```

### Install

This creates a virtual environment and installs all the keeper dependencies:

`./install.sh`

## 1\) Start virtualenv

`source _virtualenv/bin/activate`

## 2\) Create a model file

Pix a system coin/protocol token price and paste the following code into `debt_model.sh`:

```text
#!/usr/bin/env bash
while true; do
  echo "{\"price\": \"325.0\"}"
  sleep 120                   
done
```

### Then:

`chmod +x debt_model.sh`

## 3\) Create the keeper run file

Create a file called `run_auction_keeper.sh` and paste the following code in it:

```text
#!/bin/bash
bin/auction-keeper \
     --type debt \
     --model debt_model.sh \
     --rpc-uri <ETH_RPC_URL> \
     --eth-from <KEEPER_ADDRESS> \
     --eth-key key_file=<KEYSTORE_FILE>
```

### Then, substitute the following variables:

`ETH_RPC_URL` - the URL of ethereum RPC connection

`KEEPER_ADDRESS` - the keeper's address. It should be in checksummed format \(not lowercase\).

`KEYSTORE_FILE` - your Ethereum UTC JSON keystore filename

For more information about this keystore format and how to generate them, check:

* [Ethereum UTC / JSON Wallet Encryption](https://wizardforcel.gitbooks.io/practical-cryptography-for-developers-book/content/symmetric-key-ciphers/ethereum-wallet-encryption.html)
* [keythereum](https://github.com/ethereumjs/keythereum)

### Then:

`chmod +x run_auction_keeper.sh`

## 4\) Start the keeper and enter your keystore file password

`./run_auction_keeper.sh`

```text
$ ./run_auction_keeper.sh
Password for /keystore/key.json:
```

