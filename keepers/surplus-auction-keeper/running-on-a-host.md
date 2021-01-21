---
description: Running a surplus auction-keeper on a host
---

# Running on a Host

_**Not currently available on PRAI Demo**_

{% hint style="info" %}
In order to participate in surplus auctions you need to bid with protocol tokens
{% endhint %}

## Prerequisties

Python 3.6+

### Clone_**:**_

```text
git clone https://github.com/reflexer-labs/auction-keeper.git
cd auction-keeper
git checkout tags/prai-demo
git submodule update --init --recursive
```

### Install:

This creates a virtual environment and installs requirements:

`./install.sh`

## 1\) Start virtualenv

`source _virtualenv/bin/activate`

## 2\) Create a model file

Pick a protocol token/system coin price and paste the following code into `surplus_model.sh`:

```text
#!/usr/bin/env bash
while true; do
  echo "{\"price\": \"325.0\"}"
  sleep 120                   
done
```

### Then:

`chmod +x surplus_model.sh`

## 3\) Create the keeper run file

Create a file called `run_auction_keeper.sh` and paste the following code in it:

```text
#!/bin/bash
bin/auction-keeper \
     --type surplus \
     --model surplus_model.sh \
     --rpc-uri <ETH_RPC_URL> \
     --eth-from <KEEPER_ADDRESS> \
     --eth-key key_file=<KEYSTORE_FILE>
```

### Then, substitute the following variables:

`ETH_RPC_URL` - the URL of your ethereum RPC connection

`KEEPER_ADDRESS` - the keeper's address. It should be in checksummed format \(not lowercase\).

`KEYSTORE_FILE` - your Ethereum UTC JSON keystore filename

For more information about this keystore format and how to generate them, check:

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

