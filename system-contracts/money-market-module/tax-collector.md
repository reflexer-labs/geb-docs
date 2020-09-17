---
description: The protocol's taxman
---

# Tax Collector

## 1. Summary <a id="1-introduction-summary"></a>

The `TaxCollector` stores data about each `CollateralType`'s stability fee. It also collects fees from currently opened SAFEs and distributes them to the `AccountingEngine` and to other auxiliary system components such as the `StabilityFeeTreasury`.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `authorizedAccounts[usr: address]` - `addAuthorization`/`removeAuthorization`/`isAuthorized` - auth mechanisms
* `collateralTypes[collateralType: bytes32]` - mapping with data about each collateral type
* `secondaryReceiverAllotedTax[collateralType: bytes32]` - total percentage of a `CollateralType`'s fees that goes to secondary tax receivers
* `usedSecondaryReceiver[receiver: address]` - whether an address is already used as a secondary receiver
* `secondaryReceiverAccounts[receiver: address]` - whether an address is already marked as a secondary tax receiver
* `secondaryReceiverRevenueSources[receiver: address]` - number of collateral types that give part of their stability fees to secondary receivers
* `secondaryTaxReceivers[bytes32: collateralType, uint256: receiverId]` - data about a secondary receiver for a specific collateral type
* `safeEngine` - address of the `SAFEEngine`. Cannot be changed after contract deployment
* `primaryTaxReceiver` - main receiver of stability fees \(usually the `AccountingEngine`\)
* `globalStabilityFee` - base stability fee applied to all collateral types
* `secondaryReceiverNonce` - total amount of secondary receivers ever added
* `maxSecondaryReceivers` - maximum amount of secondary receivers that can receive fees from one collateral type
* `latestSecondaryReceiver` - the first secondary receiver the contract starts to loop from when distributing fees
* `collateralList` - list of all collateral types added to the collector
* `secondaryReceiverList` - list of all secondary tax receivers who receive fees from at least one collateral type

**Data Structures**

* `CollateralType`:
  * `stabilityFee` - per second fee applied to SAFEs backed by a specific asset
  * `updateTime` - latest timestamp of when the system collected fees from SAFEs backed by a specific collateral
* `TaxReceiver` - struct containing data about an auxiliary address that receives stability fees. It cannot be the `AccountingEngine` \(because it is already considered a primary tax receiver\). Contains:
  * `canTakeBackTax` - whether the system can impose a penalty on the receiver and take back previously distributed stability fees \(in case the stability fee is negative\)
  * `taxPercentage` - percentage of a collateral's stability fees distributed to this receiver

**Functions**

* `initializeCollateralType(collateralType: bytes32)` - add a new collateral type
* `modifyParameters()` - add/remove tax receivers, set stability fees etc
* `addSecondaryReceiver(collateralType: bytes32, percentage: uint256, receiver: address)`- add a new secondary receiver
* `modifySecondaryReceiver(collateral: bytes32, position: uint256, percentage: uint256)` - modify a receiver's data or remove them from the receiver list
* `collectedManyTax(start: uint256`, `end: uint256)` - check if the contract is up to date with taxation \(on multiple collateral types\)
* `taxManyOutcome(start: uint256, end: uint256) public view returns (bool ok, int rad)` - check how many fees would be collected if multiple collateral types \(between indexes `start` and `end` in `collateralList`\) would be taxed at once
* `taxSingleOutcome(collateralType: bytes32) public view returns (uint, int)` - check the amount of fees that would be collected if a single collateral type was taxed
* `secondaryReceiversAmount() public view returns (uint)` - return the amount of unique secondary receivers added to the collector
* `collateralListLength() public view returns (uint)` - return the length of `collateralList`
* `isSecondaryReceiver(receiverId: uint256) public view returns (uint)` - check if an id in the `secondaryReceiverList` was allocated to a receiver
* `taxMany(start: uint256, end: uint256)` - collect tax from multiple collateral types that are between `start` and `end` in `collateralList`
* `taxSingle(collateralType: bytes32) public returns (uint)` - collect fees from a single collateral type
* `splitTaxIncome(collateral: bytes32, debt: uint256, deltaRate: int256)` -  loop through tax receivers in order to distribute tax

#### **Events** <a id="events"></a>

* `AddAuthorization` - emitted when a new address becomes authorized. Contains:
  * `account` - the new authorized account
* `RemoveAuthorization` - emitted when an address is de-authorized. Contains:
  * `account` - the address that was de-authorized
* `CollectTax`: emitted when a collateral type is taxed \(the system collects stability fees from SAFEs backed by a specific asset\). Contains:
  * `collateralType` - collateral type being taxed
  * `latestAccumulatedRate` - accumulator of total stability fees that have been applied to`collateralType` SAFEs since the taxed collateral has been accepted into the system
  * `deltaRate` - difference between the new \(`latestAccumulatedRate`\) and the old accumulated rate for the currently taxed collateral type
* `DistributeTax` - emitted when the system distributes fees to a specific recipient. Contains:
  * `collateralType` - collateral type being taxed
  * `target` - stability fee receiver
  * `taxCut` - portion of the total tax to be distributed that goes to `target`

## 3. Walkthrough <a id="2-contract-details"></a>

The taxation process starts with governance setting up the `globalStabilityFee` and every collateral's `stabilityFee`. Governance also needs to set a `primaryTaxReceiver` \(usually the `AccountingEngine`\) and add `secondaryReceiver`s for each collateral type \(e.g the `StabilityFeeTreasury`\).

To collect stability fees, external actors \(such as protocol token holders or keepers\) can call `taxMany` \(to collect stability fees from multiple collateral types\) or `taxSingle`. The `primaryTaxReceiver` is guaranteed to receive a non zero amount of stability fees. If there are any `secondaryReceivers` set up for a specific collateral, each one will get `TaxReceiver.taxPercentage` percentage of the distributed fees.



