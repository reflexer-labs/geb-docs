---
description: Easy to use functions for redeeming collateral after settlement is triggered
---

# Proxy Global Settlement

**Smart contract code:** [GebProxyActionsGlobalSettlement](https://github.com/reflexer-labs/geb-proxy-actions/blob/b2cb42d0acb2b2489037cc19ba97da42fdf54626/src/GebProxyActions.sol#L812)

## 1. Summary <a id="1-introduction-summary"></a>

Proxy settlement functions to be used via [ds-proxy](https://github.com/reflexer-labs/ds-proxy). Meant to aid users with redeeming collateral after settlement processing is finalized. These functions assume that [`geb-cdp-manager`](https://github.com/reflexer-labs/geb-cdp-manager) is the main CDP registry.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

* `freeETH(address manager, address ethJoin, address globalSettlement, uint cdp)` - frees ETH from a CDP in `GebCDPManager` and sends it to the user's wallet
* `freeTokenCollateral(address manager, address cJoin, address settlement, uint cdp)` - frees tokens from a CDP in `GebCDPManager` and sends them to the user's wallet
* `prepareCoinsForRedeeming(address cJoin, address settlement, uint wad)` - puts system coins in a `GlobalSettlement.coinBag` by first `join`ing them in `CDPEngine`
* `redeemETH(address ethJoin, address globalSettlement, bytes32 cType, uint wad)` - redeem ETH collateral using your own bagged system coins
* `redeemTokenCollateral(address ethJoin, address settlement, bytes32 cType, uint wad)` - redeem non-ETH token collateral using your own bagged system coins

