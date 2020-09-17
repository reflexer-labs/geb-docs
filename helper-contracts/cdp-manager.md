---
description: A central hub for all SAFEs
---

# SAFE Manager

**Smart contract code:** [**GebSafeManager**](https://github.com/reflexer-labs/geb-safe-manager/blob/master/src/GebSafeManager.sol)\*\*\*\*

## 1. Summary <a id="1-introduction-summary"></a>

The SAFE Manager is an abstraction around the `SAFEEngine` that allows anyone to easily manage their GEB positions.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `safeEngine` - the address of the `SAFEEngine`
* `liquidationEngine` - the address of the `LiquidationEngine`
* `safei` - auto incrementing nonce
* `cdps[safeId: uint256]` - mapping between SAFE ids and SAFE
* `cdpList[safeId: uint256]` - double linked list indicating the previous and next SAFE ids of a provided SAFE 
* `ownsCDP[cdpId: uint256]` - indicates the owner of a SAFE
* `collateralTypes[cdpId: uint256]` - the collateral type backing the SAFE with id `safeId`
* `firstSAFEID[owner: address]` - the first `safeId` of an owner
* `lastSAFEID[owner: address]` - the last `safeId` or an owner
* `safeCount[owner: address]` - the amount of SAFEs and address has
* `safeCan[owner: address, safeId: uint256, allowedAddr: address]` - whether an address is allowed to interact with a SAFE
* `handlerCan[safeHandler: address, allowedAddr: address]` - whether an address is allowed to interact with a SAFE's handler

**Data Structures**

* `List` - a struct containing the previous and the next SAFE ids of a specific SAFE. Contains:
  * 

**Functions**

* `allowSAFE[uint256, address, uint256]` - allow an address to interact with a SAFE with a specific id
* `allowHandler[address, uint256]` - allow an address to interact with a SAFE handler
* `openSAFE[bytes32, address]` - create a new SAFE id and handler
* `transferCDPOwnership[uint256, address]` - transfer a SAFE to another address
* `modifySAFECollateralization[uint256, int256, int256]` - add/remove collateral to and from a CDP or generate/repay debt
* `transferCollateral` - transfer collateral from a SAFE to another address
* `transferInternalCoins[uint256, address, uint256]` - transfer `SAFEEngine.coinBalance` system coins between addresses
* `quitSystem[uint256, address]` - migrate the SAFE to a destination address
* `enterSystem[address, uint256]` - import a SAFE to the handler owned by an address
* `moveSAFE[uint256, uint256]` - move a position between SAFE handlers
* `protectSAFE[uint256, bytes32, address, address]` - choose a `SAFESaviour` for a SAFE

**Events**

* `NewSafe` - emitted when a new SAFE is created using `openSAFE`. Contains:
  * `usr` - the `msg.sender` when calling `openSAFE`
  * `own` - the SAFE owner
  * `safe` - SAFE id

## 3. Risks

When `openSAFE` is executed, a new `safeHandler` is created and a `safeId` is assigned to it for a specific `owner`. If the user calls `CollateralJoin.join` to add collateral to the `safeHandler` immediately after the SAFE creation transaction is mined, there is a chance that a chain reorg occurs. This would result in the user losing the ownership of the `safeHandler` and therefore lose their collateral. Users can avoid this issue when using [proxy actions](https://github.com/reflexer-labs/geb-proxy-actions/blob/master/src/GebProxyActions.sol).

