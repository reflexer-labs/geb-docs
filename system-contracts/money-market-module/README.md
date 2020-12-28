---
description: Interest rate setters and collectors
---

# Money Market Module

**Relevant smart contracts:**

* [TaxCollector](https://github.com/reflexer-labs/geb/blob/master/src/TaxCollector.sol)

## 1. Overview

The **Money Market Module** contains the components that governance \(or an autonomous rate setter\) can use to set and collect stability fees.

## 2. Component Descriptions

* The `TaxCollector` imposes fees on all collateral types and distributes them to various parties. Each collateral's stability fee is composed out of a `globalStabilityFee` \(a base fee applied to all collateral types\) and its own, unique `CollateralType.stabilityFee`.

## 3. Risks

### Smart Contract Bugs <a id="coding-errors"></a>

A bug in the `TaxCollector` would make it so that the system could no longer accrue surplus \(which is used to settle bad debt and pay for other system operations\) and thus components that depend on a constant stream of fees would likely stop working properly.

### Improper Maintenance

The `TaxCollector` depends on external actors that call `taxSingle` or `taxMany` in order to collect stability fees. Protocol token holders are incentivized to call the collector as often as possible, although in the early days of the system the calls may be more infrequent. The main caveat is that any SAFE that is opened and closed between two fee collections will not be subject to taxation.

Another major risk is related to malicious governance setting extremely high or extremely low stability fees. If the fees are extremely high, SAFEs may be unjustly liquidated. On the other hand, if they are too low, the system may not be able to collect enough fees to become sustainable.

## 4. Governance Minimization

The `TaxCollector` is part of the Level 1 Gov Minimization.

{% hint style="info" %}
**Bounded Stability Fee Control**
{% endhint %}

