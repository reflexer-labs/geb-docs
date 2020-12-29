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
* `safeCan[owner: address`, `safeId: uint256`, `allowedAddr: address]` - whether an address is allowed to interact with a SAFE
* `handlerCan[safeHandler: address`, `allowedAddr: address]` - whether an address is allowed to interact with a SAFE's handler

**Data Structures**

* `List` - a struct containing the previous and the next SAFE ids of a specific SAFE. Contains:
  * `prev` - the previous SAFE id
  * `next` - the next SAFE id

**Functions**

* `allowSAFE(safe: uint256`, `usr: address`, `ok: uint256)` - allow an address to interact with a SAFE with a specific id
* `allowHandler(usr: address`, `ok: uint256)` - allow an address to interact with a SAFE handler
* `openSAFE(collateralType: bytes32`, `usr: address)` - create a new SAFE id and handler
* `transferSAFEOwnership(safe: uint256`, `dst: address)` - transfer a SAFE to another address
* `modifySAFECollateralization(safe: uint256`, `deltaCollateral: int256`, `deltaDebt:` `int256)` - add/remove collateral to and from a SAFE or generate/repay debt
* `transferCollateral(safe: uint256`, `dst: address`, `wad: uint256)` - transfer collateral from a SAFE to another address
* `transferInternalCoins(safe: uint256`, `dst: address`, `rad: uint256)` - transfer `SAFEEngine.coinBalance` system coins between addresses
* `quitSystem(safe: uint256`, `dst: address)` - migrate the SAFE to a destination address
* `enterSystem(safe: address`, `src: uint256)` - import a SAFE to the handler owned by an address
* `moveSAFE(safeSrc: uint256`, `safeDst: uint256)` - move a position between SAFE handlers
* `protectSAFE(safe: uint256`, `liquidationEngine: address`, `saviour: address)` - choose a `SAFESaviour` for a SAFE

**Events**

* `AllowSAFE` - emitted when `allowSafe` is called. Contains:
  * `sender` - the `msg.sender`
  * `safe` - the SAFE id
  * `usr` - the address of the user that is allowed/forbidden from managing the SAFE
  * `ok` - whether the `usr` is allowed or not to manage the SAFE
* `AllowHandler` - emitted when `allowHandler` is called. Contains:
  * `sender` - the `msg.sender`
  * `usr` - the handler
  * `ok` - whether it is allowed or not
* `TransferSAFEOwnership` - emitted when `transferSAFEOwnership` is called. Contains:
  * `sender` - the `msg.sender`
  * `safe` - the SAFE id
  * `dst` - the new owner
* `OpenSAFE` - emitted when a new SAFE is created using `openSAFE`. Contains:
  * `sender` - the `msg.sender`
  * `own` - the SAFE owner
  * `safe` - SAFE id
* `ModifySAFECollateralization` - emitted when `modifySAFECollateralization` is called. Contains: 
  * `sender` - the `msg.sender`
  * `safe` - the SAFE id
  * `deltaCollateral` - the amount of collateral to add/remove
  * `deltaDebt` - the amount of debt to repay/withdraw
* `TransferCollateral` - emitted when `transferCollateral` is called. Contains:
  * `sender` - the `msg.sender`
  * `safe` - the SAFE id
  * `dst` - the destination SAFE id
  * `wad` - amount of collateral to transfer
* `TransferInternalCoins` - emitted when `transferInternalCoins` is called. Contains:
  * `sender` - the `msg.sender`
  * `safe` - the SAFE id
  * `dst` - destination for internal coins
  * `rad` - amount of internal coins to transfer
* `QuitSystem` - emitted when `quitSystem` is called. Contains:
  * `sender` - the `msg.sender`
  * `safe` - the SAFE id
  * `dst` - the destination handler for the SAFE
* `EnterSystem` - emitted when `enterSystem` is called. Contains:
  * `sender` - the `msg.sender`
  * `src` - the source handler for the SAFE
  * `safe` - the SAFE id
* `MoveSAFE` - emitted when `moveSAFE` is called. Contains:
  * `safe` - the SAFE id
  * `safeSrc` - the source ID that has the current handler controlling the SAFE
  * `safeDst` - the destination SAFE id that has the handler which will control the `safe` from now on
* `ProtectSAFE` - emitted when `protectSAFE` is called. Contains:
  * `sender` - the `msg.sender`
  * `safe` - the SAFE id
  * `liquidationEngine` - the `LiquidationEngine` where `saviour` is and that can liquidate `safe`
  * `saviour` - the saviour that will protect the SAFE

## 3. Risks

When `openSAFE` is executed, a new `safeHandler` is created and a `safeId` is assigned to it for a specific `owner`. If the user calls `CollateralJoin.join` to add collateral to the `safeHandler` immediately after the SAFE creation transaction is mined, there is a chance that a chain reorg occurs. This would result in the user losing the ownership of the `safeHandler` and therefore lose their collateral. Users can avoid this issue when using [proxy actions](https://github.com/reflexer-labs/geb-proxy-actions/blob/master/src/GebProxyActions.sol).

