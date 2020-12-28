---
description: The "source of truth" for collateral and system coin prices
---

# Oracle Module

**Relevant smart contracts:**

* [DSValue](https://github.com/reflexer-labs/ds-value/blob/master/src/value.sol)
* [OSM](https://github.com/reflexer-labs/geb-fsm/blob/master/src/OSM.sol)
* [DSM](https://github.com/reflexer-labs/geb-fsm/blob/master/src/DSM.sol)
* [GovernanceLedPriceFeedMedianizer](https://github.com/reflexer-labs/geb-governance-led-median/blob/master/src/GovernanceLedPriceFeedMedianizer.sol)
* [ChainlinkPriceFeedMedianizer](https://github.com/reflexer-labs/geb-chainlink-median/blob/master/src/ChainlinkPriceFeedMedianizer.sol)
* [OracleRelayer](https://github.com/reflexer-labs/geb/blob/master/src/OracleRelayer.sol)
* [FsmGovernanceInterface](https://github.com/reflexer-labs/geb-fsm-governance-interface/blob/master/src/FsmGovernanceInterface.sol)

## 1. Overview <a id="1-introduction-summary"></a>

The **Oracle Module** is in charge with ingesting and pushing price feed updates into the system. It has three core components: a medianizer that accepts data points from whitelisted addresses or calls multiple oracle networks, an `FSM` \(Feed Security Module\) that introduces a delay to prices coming from the medianizer and an `OracleRelayer` that divides the price data by the `redemptionPrice` and then divides the result again by the collateralization ratio \(of the asset whose price is submitted\) before pushing the final output in the `SAFEEngine`. The module may also be used to provide price feed data for the system's feedback mechanism or other contracts meant to autonomously set system parameters.

## 2. Component Descriptions

* `DSValue` is a simplified version of a medianizer. It is used for testing the oracle infrastructure. The contract creator can specify which addresses are allowed to update the price feed inside the contract.
* The `OSM` \(named via acronym from Oracle Security Module\) ensures that new price values propagated from the medianizers are not taken up by the system until a specified delay has passed.
* The `DSM` \(named via acronym from Dampened Security Module\) is an `OSM`-like contract that limits the maximum price change between two consecutive price feed updates.
* `FsmGovernanceInterface` is an abstraction meant to help governance `stop` `OSM`s.
* The `OracleRelayer` is the glue between the `OSM` and the core system \(`SAFEEngine`\). It divides every price feed by the latest `redemptionPrice` and then divides the output again by the collateralization ratio before saving the final result. The relayer will, in fact, store two different prices for each collateral type: a `safetyPrice` used only when SAFE users want to generate debt and a`liquidationPrice` used when someone calls `LiquidationEngine.liquidateSAFE`. The relayer is also in charge with storing the `redemptionPrice` and updating it using the `redemptionRate`.
* Both `GovernanceLedPriceFeedMedianizer` and `ChainlinkPriceFeedMedianizer` provide fresh price feeds for every token used in the system. The major difference between the two is that the governance led version maintains a whitelist of price feed contracts which are authorized \(and incentivized\) by token holders to push prices into the system whereas the Chainlink version does not depend on GEB's governance to function properly \(apart from instances where token holders need to point to an upgraded version of the Chainlink aggregator\).

## 3. Risks <a id="5-failure-modes-bounds-on-operating-conditions-and-external-risk-factors"></a>

* `OracleRelayer` - A bug would most likely result in the collateral prices not being updated anymore or in the `redemptionPrice` being set to an unusually high or low value.
* `GovernanceLedPriceFeedMedianizer` - there is no way to prevent a majority of the oracles to come together and sign a price of zero. This would result in the price being invalid and would return false on `getResultWithValidity`.
* `ChainlinkPriceFeedMedianizer` - governance may need to change the aggregator address in case there is an upgrade on the Chainlink infrastructure. Failure to do so will result in the price feed not being updated anymore and the need for settlement in case a solution is not found in a short period of time.
* `OSM` - governance can change the `priceSource` address to a malicious contract or to a source that does not adhere to the correct interface \(that should otherwise contain `getResultWithValidity`\). Governance may also call `stop` or `restartValue` inappropriately.
* `DSM` - can suffer from the same attacks as the `OSM`
* `FsmGovernanceInterface` - governance can maliciously stop one or more `OSM`s or `DSM`s

## 4. Governance Minimization

In the long run, governance can completely remove control over most oracle related contracts, provided that three conditions are met:

1. Governance does not plan to add any more collateral types in the future.
2. The team that deployed the system thoroughly tested its feedback mechanism, both in simulated **and** in live environments.
3. The system integrated a self-sustaining and hard to manipulate [oracle medianizer](https://docs.reflexer.finance/system-contracts/oracle-module/medianizer/oracle-network-medianizer) that provides price feeds for both the core system coin and for all collateral types.

