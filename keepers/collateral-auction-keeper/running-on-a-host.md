# Running a Collateral Auction Keeper on a Host

## Prerequisties
Python 3.6+

#### Get RAI

Buy RAI from [Uniswap v2](https://info.uniswap.org/pair/0xEBdE9F61e34B7aC5aAE5A4170E964eA85988008C) or 
[open a SAFE](https://app.gitbook.com/@reflexer-labs/s/geb/pyflex/safe-management/opening-a-safe) and generate it.


#### Clone
```
git clone https://github.com/reflexer-labs/auction-keeper.git
cd auction-keeper
git checkout tags/prai-demo
git submodule update --init --recursive
```

#### Install
This creates a virtual environment and installs requirements.

`./install.sh`

## 1) Start virtualenv

```source _virtualenv/bin/activate```

## 2) Create a model file 

Paste the following code into `collateral_model.sh`.  

```
#!/usr/bin/env bash
while true; do
  echo "{}"
  sleep 120                   
done
```
NOTE: Collateral auctions sell collateral at a fixed discount, so the keeper doesn't use a bidding model.  This 'empty' bidding model is simply a placeholder.

## 3) Create the keeper run file.

Create a file called  `run_auction_keeper.sh` and paste the following code in it:

```text
#!/bin/bash
bin/auction-keeper \
     --model ./collateral_model.sh \
     --rpc-uri <ETH_RPC_URL> \
     --eth-from <KEEPER_ADDRESS> \
     --eth-key key_file=<KEYSTORE_FILE>       
```

### Then, substitute the following variables:

`ETH_RPC_URL` - The URL of ethereum RPC connection

`KEEPER_ADDRESS` - The keeper's address. It should be in checksummed format(not lowercase).

`KEYSTORE_FILE` - Your Ethereum UTC JSON keystore filename

For more information about this keystore format and how to generate them:

* [Ethereum UTC / JSON Wallet Encryption](https://wizardforcel.gitbooks.io/practical-cryptography-for-developers-book/content/symmetric-key-ciphers/ethereum-wallet-encryption.html)

* [keythereum](https://github.com/ethereumjs/keythereum)

### Then
`chmod +x run_auction_keeper.sh`

## 4) Start the keeper and enter your keystore file password

`./run_auction_keeper.sh`

```text
$ ./run_auction_keeper.sh
Password for /keystore/key.json: 
```
