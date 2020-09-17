---
description: Execute transactions with the use of a proxy
---

# DSProxy

**Smart contract code:** [**DSProxy**](https://github.com/reflexer-labs/ds-proxy/blob/master/src/proxy.sol)\*\*\*\*

## 1. Summary <a id="1-introduction-summary"></a>

A user can execute functions through this proxy by passing in the bytecode for the target contract as well as the calldata for the function they want to call.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `cache` - address of contract that caches the bytecode of target contracts called by the proxy

**Functions**

* `execute` - execute a function in the context of the proxy
* `setCache` - set a new `cache`

