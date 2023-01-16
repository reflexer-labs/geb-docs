---
description: Simple price feed setter and getter
---

# DSValue

## 1. Summary <a id="1-introduction"></a>

This is a simple contract where authorized addresses can set a price and anyone can read it.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `isValid` - boolean that signals whether the currently stored value is valid \(greater than zero\) or not
* `medianPrice` - the current price feed

**Modifiers**

* `auth` - modifier that checks whether an address can set the `result`. Inherited from [ds-thing](https://github.com/dapphub/ds-thing).

**Functions**

* `getResultWithValidity() external view returns (bytes32, bool)` - returns `result` and `isValid`
* `read() external view returns (uint256)` - getter that only returns the `result`
* `updateResult(newResult: bytes32)` - set a new `result`
* `restartValue()` - set `isValid` to `false`

## 3. Walkthrough <a id="3-key-mechanisms-and-concepts"></a>

Authorized functions can set a new `medianPrice` by calling `updateResult`. Anyone can `read` the `medianPrice` or read both the `medianPrice` and whether it `isValid` by calling `read` or`getResultWithValidity`.

## 4. Gotchas

This oracle is entirely dependent on authed addresses to update them. They will not flag if they are stale.

## 5. Failure Modes (Bounds on Operating Conditions & External Risk Factors)

Authed address can set any arbitrary price. This will impact other components reading from the oracle.

