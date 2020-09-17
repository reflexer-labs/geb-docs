---
description: Tracker and builder of DSProxy contracts
---

# Proxy Registry

**Smart contract code:** [**GebProxyRegistry**](https://github.com/reflexer-labs/geb-proxy-registry/blob/master/src/GebProxyRegistry.sol)\*\*\*\*

## 1. Summary <a id="1-introduction-summary"></a>

This is a registry of already created DSProxies.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `proxies` - mapping of already created proxies \(for different addresses\)
* `factory` - contract that creates DSProxies

**Functions**

* `build` - create a new DSProxy

