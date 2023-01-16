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

### `rpow`

`rpow(uint x, uint n, uint b)`, used for exponentiation in `taxSingle/taxMany`, is a fixed-point arithmetic function that raises `x` to the power `n`. It is implemented in Solidity assembly as a repeated squaring algorithm. `x` and the returned value are to be interpreted as fixed-point integers with scaling factor `b`. For example, if `b == 100`, this specifies two decimal digits of precision and the normal decimal value 2.1 would be represented as 210; `rpow(210, 2, 100)` returns 441 \(the two-decimal digit fixed-point representation of 2.1^2 = 4.41\). In the current implementation, 10^27 is passed for `b`, making `x` and the `rpow` result both of type `ray` in standard GEB fixed-point terminology. `rpow`'s formal invariants include "no overflow" as well as constraints on gas usage.

### Parameters Can Only Be Set By Governance

TaxCollector stores some sensitive parameters, particularly the base rate and collateral-specific risk premiums that determine the overall stability fee rate for each collateral type. Its built-in authorization mechanisms need to allow only authorized governance contracts/actors to set these values. See "Failure Modes" for a description of what can go wrong if parameters are set to unsafe values.

## 4. Gotchas \(Potential Sources of User Error\)

### Collateral Initialization

`initializeCollateralType(bytes32 collateral)` must called when a new collateral is added \(setting `stabilityFee` via `modifyParameters()` is not sufficient\)—otherwise `updateTime` will be uninitialized and fees will accumulate based on a start date of January 1st, 1970 \(start of Unix epoch\).

### `globalStabilityFee + CollateralType.stabilityFee` imbalance in `taxSingle()`

A call to `taxSingle(bytes32 collateral)`will add the `globalStabilityFee` rate to the `CollateralType.stabilityFee` rate. The rate is a calculated compounded rate, so `rate(globalStabilityFee + CollateralType.stabilityFee) != rate(globalStabilityFee) + rate(CollateralType.stabilityFee)`. This means that if globalStabilityFee is set, the CollateralType.stabilityFee will need to be set factoring the existing compounding factor in globalStabilityFee, otherwise the result will be outside of the rate tolerance. Updates to the `globalStabilityFee` value will require all of the `CollateralTypes` to be updated as well.

## 5. Failure Modes \(Bounds on Operating Conditions & External Risk Factors\)

### Tragedy of the Commons

If `taxSingle()` is called very infrequently for some collateral types \(due, for example, to low overall system usage or extremely stable collateral types that have essentially zero liquidation risk\), then the system will fail to collect fees on SAFES opened and closed between `taxSingle()` calls. As the system achieves scale, this becomes less of a concern, as both Keepers and FLX holders are have an incentive to regularly call taxSingle \(the former to trigger liquidation auctions, the latter to ensure that surplus accumulates to decrease FLX supply\); however, a hypothetical asset with very low volatility yet high risk premium might still see infrequent taxSingle calls at scale \(there is not at present a real-world example of this—the most realistic possibility is `globalStabilityFee` being large, elevating rates for all collateral types\).

### Malicious or Careless Parameter Setting

Various parameters of TaxCollector may be set to values that damage the system. While this can occur by accident, the greatest concern is malicious attacks, especially by an entity that somehow becomes authorized to make calls directly to TaxCollector's administrative methods, bypassing governance. Setting `stabilityFee` \(for at least one collateral\) or `globalStabilityFee` too low can lead to Dai oversupply; setting either one too high can trigger excess liquidations and therefore unjust loss of collateral. Setting a value for `accountingEngine` other than the true AccountingEngine's address can cause surplus to be lost or stolen.

