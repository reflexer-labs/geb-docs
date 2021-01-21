---
description: Running a collateral auction keeper directly on a host
---

# Running on a Host

## Prerequisties

Python 3.6+

### Get RAI:

Buy RAI from Uniswap v2 or [open a SAFE](https://app.gitbook.com/@reflexer-labs/s/geb/pyflex/safe-management/opening-a-safe) and generate it.

### Clone:

```text
git clone https://github.com/reflexer-labs/auction-keeper.git
cd auction-keeper
git checkout tags/prai-demo
git submodule update --init --recursive
```

### Install:

This creates a virtual environment and installs requirements.

`./install.sh`

## 1\) Start virtualenv

`source _virtualenv/bin/activate`

## 2\) Create a model file

Paste the following code into `collateral_model.sh`:

```text
#!/usr/bin/env bash
while true; do
  echo "{}"
  sleep 120                   
done
```

**NOTE**: Currently, collateral auctions sell collateral at a fixed discount and so the keeper doesn't use a bidding model. This empty bidding model is simply a placeholder.

## 3\) Create the keeper run file

Create a file called `run_auction_keeper.sh` and paste the following code in it:

```text
#!/bin/bash
bin/auction-keeper \
     --model ./collateral_model.sh \
     --rpc-uri <ETH_RPC_URL> \
     --eth-from <KEEPER_ADDRESS> \
     --eth-key key_file=<KEYSTORE_FILE>
```

### Then, substitute the following variables:

`ETH_RPC_URL` - the URL of the ethereum RPC connection

`KEEPER_ADDRESS` - the keeper's address. It should be in checksummed format\(not lowercase\).

`KEYSTORE_FILE` - your Ethereum UTC JSON keystore filename

For more information about this keystore format and how to generate them:

* [Ethereum UTC / JSON Wallet Encryption](https://wizardforcel.gitbooks.io/practical-cryptography-for-developers-book/content/symmetric-key-ciphers/ethereum-wallet-encryption.html)
* [keythereum](https://github.com/ethereumjs/keythereum)

### Finally:

`chmod +x run_auction_keeper.sh`

## 4\) Start the keeper and enter your keystore file password

`./run_auction_keeper.sh`

```text
$ ./run_auction_keeper.sh
Password for /keystore/key.json:
```

## Collateral Auctioning Process

[Collateral Auctioning Process](liquidations.md)

[Flash Swaps](flash-swaps.md)
