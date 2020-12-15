---
description: Information about bidding models for debt and surplus auction keepers.
---

# Bidding Models

`auction-keeper` maintains a collection of child processes, as each bidding model is its own dedicated process. New processes \(new _bidding model_ instances\) are spawned by executing the command passed to `--model`. These processes are automatically terminated \(via `SIGKILL`\) by the keeper shortly after their associated auction expires.

Whenever the _bidding model_ process dies, it gets automatically respawned by the keeper.

Example:

```bash
bin/auction-keeper --model '../my-bidding-model.sh' [...]
```

## Pass auction status to each bidding model

`auction-keeper` communicates with bidding models via their standard input and standard output. When the auction state changes, the keeper sends a one-line JSON document to the **standard input** of the bidding model process.

Sample message sent from the keeper to the model looks like:

```javascript
{"id": "6", "surplus_auction_house": "0xf0afc3108bb8f196cf8d076c8c4877a4c53d4e7c", "bid_amount": "7.142857142857142857", "amount_to_sell": "10000.000000000000000000", "bid_increase": "1.050000000000000000", "high_bidder": "0x00531a10c4fbd906313768d277585292aa7c923a", "era": 1530530620, "bid_expiry": 1530541420, "auction_deadline": 1531135256, "price": "1400.000000000000000028"}
```

Bidding models should never make an assumption that messages will be sent only when auction state changes.

At the same time, the `auction-keeper` reads one-line messages from the **standard output** of the bidding model process and tries to parse them as JSON documents. Then it extracts two fields from that document:

* `price` - the maximum \(for debt auctions\) or the minimum \(for surplus auctions\) price

  the model is willing to bid. This value is ignored for fixed discount collateral auctions

* `gasPrice` \(optional\) - gas price in Wei to use when sending the bid.

## Processing each bidding model output and submitting bids

### Sample model output from a Debt Auction bidding model

A sample message sent from the debt model to the keeper may look like:

`price` is `PROT/System Coin` price

```javascript
{"price": "250.0", "gasPrice": 70000000000}
```

### Sample model output from a Surplus Auction bidding model

A sample message sent from the debt model to the keeper may look like:

`price` is `PROT/System Coin` price

```javascript
{"price": "150.0"}
```

Any messages written by a _bidding model_ to **stderr** will be passed through by the keeper to its logs. This is the most convenient way of implementing logging from _bidding models_.

**Currently no utility is provided to prevent you from bidding at an unprofitable price.**

## Simplest possible bidding model

```text
#!/usr/bin/env bash
while true; do
  echo "{\"price\": \"723.0\"}"
  sleep 120                   
done
```

Specifying a gas price is optional. If you want to start with a fixed gas price, you can add it like this:

```text
#!/usr/bin/env bash

while true; do
  echo "{\"price\": \"723.0\", \"gasPrice\": \"70000000000\"}"    # put your desired gas price in Wei here
  sleep 120                                                       # locking the gas price for n seconds
done
```

The model produces price\(s\) for the keeper. After the `sleep` period. the keeper will restart the price model and read new price\(s\).  
Consider this your price update interval.

## Collateral bidding models

**Note:** Collateral keepers buy collateral at a fixed discount and don't bid prices. So they use a blank model file.

```text
#!/usr/bin/env bash
while true; do
  echo "{}"
  sleep 120
done
```

## Other bidding models

Thanks to our community for these examples:

* _banteg_'s [python boilerplate model](https://gist.github.com/banteg/93808e6c0f1b9b6b470beaba5a140813)

