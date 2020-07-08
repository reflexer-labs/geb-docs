---
description: A central hub for all CDPs
---

# CDP Manager

**Smart contract code:** [**GebCdpManager**](https://github.com/reflexer-labs/geb-cdp-manager/blob/master/src/GebCdpManager.sol)\*\*\*\*

## 1. Summary <a id="1-introduction-summary"></a>

The CDP Manager is an abstraction around the `CDPEngine` that allows anyone to easily manage their GEB positions.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

* `cdpEngine` - the address of the `CDPEngine`
* `liquidationEngine` - the address of the `LiquidationEngine`
* `cdpi` - auto incrementing nonce
* `cdps[cdpId: uint256]` - mapping between CDP ids and CDP handlers
* `cdpList[cdpId: uint256]` - double linked list indicating the previous and next CDP ids of a provided CDP 
* `ownsCDP[cdpId: uint256]` - indicates the owner of a CDP
* `collateralTypes[cdpId: uint256]` - the collateral type backing the CDP with id `cdpId`
* `firstCDPID[owner: address]` - the first `cdpId` of an owner
* `lastCDPID[owner: address]` - the last `cdpId` or an owner
* `cdpCount[owner: address]` - the amount of CDPs and address has
* `cdpCan[owner: address, cdpId: uint256, allowedAddr: address]` - whether an address is allowed to interact with a CDP
* `handlerCan[cdpHandler: address, allowedAddr: address]` - whether an address is allowed to interact with a CDP's handler
* `List` - a struct containing the previous and the next CDP ids of a specific CDP
* `allowCDP[uint256, address, uint256]` - allow an address to interact with a CDP with a specific id
* `allowHandler[address, uint256]` - allow an address to interact with a CDP handler
* `openCDP[bytes32, address]` - create a new CDP id and handler
* `transferCDPOwnership[uint256, address]` - transfer a CDP to another address
* `modifyCDPCollateralization[uint256, int256, int256]` - add/remove collateral to and from a CDP or generate/repay debt
* `transferCollateral` - transfer collateral from a CDP to another address
* `transferInternalCoins[uint256, address, uint256]` - transfer `CDPEngine.coinBalance` system coins between addresses
* `quitSystem[uint256, address]` - migrate the CDP to a destination address
* `enterSystem[address, uint256]` - import a CDP to the handler owned by an address
* `moveCDP[uint256, uint256]` - move a position between CDP handlers
* `protectCDP[uint256, bytes32, address, address]` - choose a `CDPSaviour` for a CDP

**Events**

* `NewCdp` - emitted when a new CDP is created using `openCDP`. Contains:
  * `usr` - the `msg.sender` when calling `openCDP`
  * `own` - the CDP owner
  * `cdp` - CDP id

## 3. Risks

When `openCDP` is executed, a new `cdpHandler` is created and a `cdpId` is assigned to it for a specific `owner`. If the user calls `CollateralJoin.join` to add collateral to the `cdpHandler` immediately after the CDP creation transaction is mined, there is a chance that a chain reorg occurs. This would result in the user losing the ownership of the `cdpHandler` and therefore lose their collateral. Users can avoid this issue when using [proxy actions](https://github.com/reflexer-labs/geb-proxy-actions/blob/master/src/GebProxyActions.sol).

