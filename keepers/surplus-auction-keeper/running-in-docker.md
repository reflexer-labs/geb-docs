---
description: Running a surplus auction keeper in a Docker container
---

# Running in Docker

{% hint style="info" %}
In order to participate in surplus auctions you need to bid with protocol tokens
{% endhint %}

## 1\) Modify model file as needed

A basic surplus auction bidding model can be found in `models/surplus_model.py`.
 It **must** be modifed to change `CURRENT_FLX_USD_PRICE`, which is currently set as a constant. However, the script can be modified to fetch the latest protocol token price from an external source.

The minimum acceptable profit when bidding is determined by `MINIMUM_FLX_MULTIPLIER` in the model file and is set to `1.10`. This means the lowest price(in RAI) you will accept for protocol tokens is 110% of `CURRENT_FLX_USD_PRICE`.

### Then:

`chmod +x surplus_model.py`

For more information about bidding models, see [Bidding Models](../BiddingModels.md)

## 2\) Modify keeper run file

Modify the following variables in `run_surplus_keeper.sh`


`KEEPER_ADDRESS` - the keeper's address. It should be in checksummed format \(not lowercase\)

`ETH_RPC_URL` - the URL of your Ethereum RPC connection

`KEYSTORE_DIR` - the full path of the directory where your keystore file is

`MODEL_DIR` - the full path of directory where your `surplus_model.py` file is

`KEYSTORE_FILE` - your Ethereum UTC JSON keystore filename

For more information about this keystore format and how to generate them:

* [Ethereum UTC / JSON Wallet Encryption](https://wizardforcel.gitbooks.io/practical-cryptography-for-developers-book/content/symmetric-key-ciphers/ethereum-wallet-encryption.html)
* [keythereum](https://github.com/ethereumjs/keythereum)


### Then:

`chmod +x run_surplus_keeper.sh`

## 4\) Start the keeper and enter your keystore file password

`./run_surplus_keeper.sh`

```text
$ ./run_auction_keeper.sh
Pulling from reflexer/auction-keeper
Digest: sha256:7e55ec9b0a136fc903d9f7f2690538bcbde9029d957e0e6f84d0282790f9666a
Status: Downloaded newer image for reflexer/auction-keeper
docker.io/reflexer/auction-keeper
Password for /keystore/key.json:
```

## Surplus Auctioning Process

[Surplus Auctioning Process](surplus-auctions.md)
