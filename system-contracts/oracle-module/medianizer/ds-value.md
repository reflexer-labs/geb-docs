---
description: Simple price feed setter and getter
---

# DSValue

## 1. Summary <a id="1-introduction"></a>

This is a simple contract where authorized addresses can set a price and anyone can read it.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

* `auth` - modifier that checks whether an address can set the `result`. Inherited from [ds-thing](https://github.com/dapphub/ds-thing).
* `isValid` - boolean that signals whether the currently stored value is valid \(greater than zero\) or not
* `medianPrice` - the current price feed
* `getResultWithValidity` - returns `result` and `isValid`
* `read` - getter that only returns the `result`
* `updateResult(bytes32: newResult)` - set a new `result`
* `restartValue` - set `isValid` to `false`

## 3. Walkthrough <a id="3-key-mechanisms-and-concepts"></a>

Authorized functions can set a new `medianPrice` by calling `updateResult`. Anyone can `read` the `medianPrice` or read both the `medianPrice` and whether it `isValid` by calling `read` or`getResultWithValidity`.

