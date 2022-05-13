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

## 2\) Modify model file as needed

A basic debt auction bidding model can be found in `models/debt_model.py`.
This model retrieves the latest FLX/USD price from coingecko and will automatically place bids in an auction.

You probably want to modify the following variables in `models/surplus_model.py`:

`MAXIMUM_FLX_MULTIPLIER`: The maximum acceptable FLX price to use when bidding. Default: `0.90` meaning the maximum price to pay when buying FLX(in RAI) is 90% of the current FLX/USD market price

`MY_BID_DECREASE`: The amount of bid decrease(in FLX) to make when outbidding another bidder. If value is less than the auction house' `bidDncrease`, then it will use the auction house setting. Example: A value of `1.10` will create bid decreases of 10%. Note: Current `bidDncrease` on mainnet is `1.03`. Default: `1.03`

### Then:

`chmod +x debt_model.py`

For more information about bidding models, see [Bidding Models](../BiddingModels.md)

## 3\) Modify keeper run file

Modify the following variables in `run_debt_keeper_host.sh`


`KEEPER_ADDRESS` - the keeper's address. It should be in checksummed format \(not lowercase\)

`ETH_RPC_URL` - the URL of your Ethereum RPC connection

`KEYSTORE_DIR` - the full path of the directory where your keystore file is

`MODEL_DIR` - the full path of directory where your `surplus_model.py` file is

`KEYSTORE_FILE` - your Ethereum UTC JSON keystore filename

For more information about this keystore format and how to generate them:

* [Ethereum UTC / JSON Wallet Encryption](https://wizardforcel.gitbooks.io/practical-cryptography-for-developers-book/content/symmetric-key-ciphers/ethereum-wallet-encryption.html)
* [keythereum](https://github.com/ethereumjs/keythereum)

`GAS_MAXIMUM` -maximum gas price, in GWEI

### Then:

`chmod +x run_debt_keeper_host.sh`

## 4\) Start the keeper and enter your keystore file password

`./run_debt_keeper_host.sh`

```text
$ ./run_debt_keeper_host.sh
Password for /keystore/key.json:
```

