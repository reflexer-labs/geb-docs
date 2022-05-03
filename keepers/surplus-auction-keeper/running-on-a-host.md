---
description: Running a surplus auction-keeper on a host
---

# Running on a Host

{% hint style="info" %}
In order to participate in surplus auctions you need to bid with protocol tokens
{% endhint %}

## Prerequisties

Python 3.6+

### Clone_**:**_

```text
git clone https://github.com/reflexer-labs/auction-keeper.git
cd auction-keeper
git submodule update --init --recursive
```

### Install:

This creates a virtual environment and installs requirements:

`./install.sh`

## 1\) Start virtualenv

`source _virtualenv/bin/activate`

## 2\) Modify model file as needed

A basic surplus auction bidding model can be found in `models/surplus_model.py`.
This model retrieves the latest FLX/USD price from coingecko and will automatically place bids in an auction.

You probably want to modify the following variables in `models/surplus_model.py`:

`MAXIMUM_FLX_MULTIPLIER`: The maximum acceptable FLX price to use when bidding. This value will be used when bidding on a new auction with no previous bids. Default: `1.50` meaning the maxiimum price to accept for FLX(in RAI) is 150% of the current FLX/USD market price


`MINIMUM_FLX_MULTIPLIER`: The minimum acceptable FLX price to use when bidding. Default: `1.10` meaning the minimum price to accept for FLX(in RAI) is 110% of the current FLX/USD market price

`MY_BID_INCREASE`: The amount of bid increase(in FLX) to make when outbidding another bidder. If value is less than the auction house' `bidIncrease`, then it will use the auction house setting. Note: Current `bidIncrease` on mainnet is `1.03`. Default: `1.10`, If you want to always use the auction house `bidIncrease`, set this to `0`.

### Ensure script is executable

`chmod +x surplus_model.py`

For more information about bidding models, see [Bidding Models](../BiddingModels.md)

## 3\) Modify keeper run file

Modify the following variables in `run_surplus_keeper_host.sh`

`KEEPER_ADDRESS` - the keeper's address. It should be in checksummed format \(not lowercase\).

`ETH_RPC_URL` - the URL of your ethereum RPC connection

`KEYSTORE_FILE` - your Ethereum UTC JSON keystore filename

For more information about this keystore format and how to generate them, check:

* [Ethereum UTC / JSON Wallet Encryption](https://wizardforcel.gitbooks.io/practical-cryptography-for-developers-book/content/symmetric-key-ciphers/ethereum-wallet-encryption.html)
* [keythereum](https://github.com/ethereumjs/keythereum)

`GAS_MAXIMUM` -maximum gas price, in GWEI

### Ensure script is executable

`chmod +x run_surplus_keeper_host.sh`

## 4\) Start the keeper and enter your keystore file password

`./run_surplus_keeper_host.sh`

```text
$ ./run_surplus_keeper_host.sh
Password for /keystore/key.json:
```

## Surplus Auctioning Process

[Surplus Auctioning Process](surplus-auctions.md)
