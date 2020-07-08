---
description: The protocol's taxman
---

# Tax Collector

## 1. Summary <a id="1-introduction-summary"></a>

The `TaxCollector` stores data about each `CollateralType`'s stability fee. It also collects fees from currently opened CDPs and distributes them to the `AccountingEngine` and to other auxiliary system components such as the `StabilityFeeTreasury`.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

* `authorizedAccounts[usr: address]` - `addAuthorization`/`removeAuthorization`/`isAuthorized` - auth mechanisms
* `CollateralType`:
  * `stabilityFee` - per second fee applied to CDPs backed by a specific asset
  * `updateTime` - latest timestamp of when the system collected fees from CDPs backed by a specific collateral
* `TaxReceiver` - auxiliary address that receives stability fees. It cannot be the `AccountingEngine` \(because it is already considered a primary tax receiver\). Contains:
  * `canTakeBackTax` - whether the system can impose a penalty on the receiver and take back previously distributed stability fees \(in case `globalStabilityFee + CollateralType.stabilityFee` is negative\)
  * `taxPercentage` - percentage of `globalStabilityFee + CollateralType.stabilityFee` distributed to this receiver
* `collateralTypes[collateralType: bytes32]` - mapping with data about each collateral type
* `secondaryReceiverAllotedTax[collateralType: bytes32]` - total percentage of a `CollateralType`'s fees that goes to secondary tax receivers
* `usedSecondaryReceiver[receiver: address]` - whether an address is already used as a secondary receiver
* `secondaryReceiverAccounts[receiver: address]` - whether an address is already marked as a receiver
* `secondaryReceiverRevenueSources[receiver: address]` - amount of collateral types that give part of their stability fees to a secondary receiver
* `secondaryTaxReceivers[bytes32: collateralType, uint256: receiverId]` - data about a secondary receiver for a specific collateral type
* `cdpEngine` - address of the `CDPEngine`. Cannot be changed after contract deployment
* `primaryTaxReceiver` - main receiver of stability fees \(usually the `AccountingEngine`\)
* `globalStabilityFee` - base stability fee applied to all collateral types
* `secondaryReceiverNonce` - total amount of secondary receivers ever added
* `maxSecondaryReceivers` - maximum amount of secondary receivers that can receive fees from one collateral type
* `latestSecondaryReceiver` - the first secondary receiver the contract starts to loop from when distributing fees
* `collateralList` - list of all collateral types added to the collector
* `secondaryReceiverList` - list of all secondary tax receivers who receive fees from at least one collateral type
* `initializeCollateralType(collateralType: bytes32)` - add a new collateral type
* `modifyParameters` - add/remove tax receivers, set stability fees etc
* `addSecondaryReceiver(collateralType: bytes32, percentage: uint256, receiver: address)`- add a new secondary receiver
* `modifySecondaryReceiver(collateral: bytes32, position: uint256, percentage: uint256)` - modify a receiver's data or remove them from the receiver list
* `collectedAllTax` - check if the contract is up to date with taxation \(on all collateral types\)
* `taxManyOutcome(start: uint256, end: uint256)` - check how many fees would be collected if multiple collateral types \(between indexes `start` and `end` in `collateralList`\) would be taxed at once
* `taxSingleOutcome(collateralType: bytes32)` - check the amount of fees that would be collected if a single collateral type was taxed
* `secondaryReceiversAmount` - return the amount of unique secondary receivers added to the collector
* `collateralListLength` - return the length of `collateralList`
* `isSecondaryReceiver(receiverId: uint256)` - check if an id in the `secondaryReceiverList` was allocated to a receiver
* `taxMany(start: uint256, end: uint256)` - collect tax from multiple collateral types that are between `start` and `end` in `collateralList`
* `taxSingle(collateralType: bytes32)` - collect fees from a single collateral type
* `splitTaxIncome(collateral: bytes32, debt: uint256, deltaRate: int256)` -  loop through tax receivers in order to distribute tax
* `distributeTax` - distribute tax to a specific address

#### **Events** <a id="events"></a>

* `CollectTax`: emitted when a collateral type is taxed \(the system collects stability fees from CDPs backed by a specific asset\). Contains:
  * `collateralType` - collateral type being taxed
  * `latestAccumulatedRate` - accumulator of total stability fees that have been applied to`collateralType` CDPs since the taxed collateral has been accepted into the system
  * `deltaRate` - difference between the new \(`latestAccumulatedRate`\) and the old accumulated rate for the currently taxed collateral type
* `DistributeTax` - emitted when the system distributes fees to a specific recipient. Contains:
  * `collateralType` - collateral type being taxed
  * `target` - stability fee receiver
  * `taxCut` - portion of the total tax to be distributed that goes to `target`

## 3. Walkthrough <a id="2-contract-details"></a>

The taxation process starts with governance setting up the `globalStabilityFee` and every collateral's `stabilityFee`. Governance also needs to set a `primaryTaxReceiver` \(usually the `AccountingEngine`\) and add `secondaryReceiver`s for each collateral type \(e.g the `StabilityFeeTreasury`\).

To collect stability fees, external actors \(such as protocol token holders or keepers\) can call `taxMany` \(to collect stability fees from multiple collateral types\) or `taxSingle`. The `primaryTaxReceiver` is guaranteed to receive a non zero amount of stability fees. If there are any `secondaryReceivers` set up for a specific collateral, each one will get `TaxReceiver.taxPercentage` percentage of the distributed fees.



