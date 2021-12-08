---
description: Chainlink integrated medianizer
---

# Chainlink Median

## 1. Summary <a href="#1-introduction" id="1-introduction"></a>

The `ChainlinkPriceFeedMedianizer` has a similar interface to the [Governance Led Median](https://reflexer-labs.gitbook.io/geb/system-contracts/untitled-1/medianizer/governance-led) although, instead of relying on governance whitelisted oracles, it simply keeps a reference to a [Chainlink price reference contract](https://feeds.chain.link) (price aggregator).

## 2. Contract Variables & Functions <a href="#2-contract-details" id="2-contract-details"></a>

**Variables**

* `authorizedAccounts [usr: address]` - `addAuthorization`/`removeAuthorization`/`isAuthorized` - auth mechanisms
* `staleThreshold` - time since `linkAggregatorTimestamp` after which the median value is considered stale&#x20;
* `chainlinkAggregator` - address of the Chainlink price reference contract
* `rewardRelayer` - address of the contract that rewards addresses that call `updateResult`
* `medianPrice` - latest fetched price&#x20;
* `lastUpdateTime` - latest timestamp when the contract pulled a price update from Chainlink
* `multiplier` - scaling factor for the Chainlink result (e.g if the price has 8 decimals and we want it to be scaled to 18 decimals, `multiplier = 10` and `medianPrice = fetchedPrice * 10 ^ multiplier`)
* `symbol` - the price oracle type (ex: ETHUSD)
* `periodSize` - the minimum delay between two consecutive updates after which the reward for updating again starts to increase
* `linkAggregatorTimestamp` - the timestamp of the Chainlink aggregator's latest price update

**Modifiers**

* `isAuthorized` **** - checks whether an address is part of `authorizedAddresses` (and thus can call authed functions).

**Functions**

* `modifyParameters` - allows governance to change contract parameters
* `read() public view returns (uint256)` - gets a non-zero price or fails
* `getResultWithValidity() public view returns (uint256,bool)` - gets the price and its validity
* `updateResult(feeReceiver: address)` - updates the price stored in the contract by calling the Chainlink aggregator

**Events**

* `AddAuthorization` - emitted when a new address becomes authorized. Contains:
  * `account` - the new authorized account
* `RemoveAuthorization` - emitted when an address is de-authorized. Contains:
  * `account` - the address that was de-authorized
* `ModifyParameters` **** - emitted when a parameter is updated
* `UpdateResult` - emitted when `updateResult` is called. Contains:
  * `medianPrice` - the latest median price
  * `lastUpdateTime` - timestamp of the call

## 3. Walkthrough

When reading the latest price feed, the contract also stores the timestamp when the price coming from Chainlink was posted on-chain. The system can incentivize anyone to call `updateResult` and update the median price regularly using a `rewardRelayer`.
