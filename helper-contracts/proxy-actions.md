---
description: Bundling together core GEB functions for ease of use
---

# Proxy Actions

**Smart contract code:** [GebProxyActions](https://github.com/reflexer-labs/geb-proxy-actions/blob/99cf6b3f6c3e214a3b7d7048b7bd28405d4f103a/src/GebProxyActions.sol#L138)

## 1. Summary <a id="1-introduction-summary"></a>

Proxy core functions to be used via [ds-proxy](https://github.com/reflexer-labs/ds-proxy). These functions assume that [`geb-cdp-manager`](https://github.com/reflexer-labs/geb-cdp-manager) is the main CDP registry.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

* `transfer(address collateral, address dst, uint amt)` - transfer system collateral to a `dst` address
* `ethJoin_join(address apt, address cdp)` - `join` ETH collateral inside `CDPEngine`
* `tokenCollateralJoin_join(address apt, address cdp, uint amt, bool transferFrom)` - join token collateral inside `CDPEngine`. Will `transferFrom` only if the token has approval/transfer in its implementation
* `approveCDPModification(address obj, address usr)` - approve another address to access your CDP inside `CDPEngine`
* `denyCDPModification(address obj, address usr)` - deny an address the right to access your CDP inside `CDPEngine` 
* `openCDP(address manager, bytes32 collateralType, address usr)` - open a new CDP \(only\) inside a `CDPManager`
* `transferCDPOwnership(address manager, uint cdp, address usr)` - transfer a CDP to another address \(only inside a `CDPManager`, not inside `CDPEngine`\)
* `giveToProxy(address proxyRegistry, address manager, uint cdp, address usr)` - transfer a CDP's ownership to your own proxy. The transfer only happens inside a `CDPManager` and not in `CDPEngine`
* `allowCDP(address manager, uint cdp, address usr, uint ok)` - allow/deny access to a CDP with id `cdp` inside `CDPManager`
* `allowHandler(address manager, address usr, uint ok)` - allow/deny access to a CDP's handler \(CDP's address inside `CDPEngine`\) only in `CDPManager`
* `transferCollateral(address manager, uint cdp, address dst, uint wad)` - transfer collateral between CDPs that are in `CDPManager`
* `transferInternalCoins(address manager, uint cdp, address dst, uint rad)` - transfer internal coins \(inside `CDPEngine`\) from one address to another
* `modifyCDPCollateralization(address manager, uint cdp, int dCollateral, int dDebt)` - add collateral and/or generate/pay back debt using a CDP created inside `CDPManager`
* `quitSystem(address manager, uint cdp, address dst)` - migrate the CDP to a destination address
* `enterSystem(address manager, uint cdp, address dst)` - import a CDP to the handler owned by an address
* `moveCDP(address manager, uint cdpSrc, uint cdpDst)` - move a position between CDP handlers
* `makeCollateralBag(address collateralJoin)` - used for tokens that do not have `transferFrom` implemented \(e.g GNT\). Creates a token bag where the user can add collateral and then "join" the bag inside `CollateralJoin` 
* `lockETH(address manager, address ethJoin, uint cdp)` - add more ETH into a CDP created with a `CDPManager`
* `safeLockETH(address manager, address ethJoin, uint cdp, address owner)` - first checks to see if the `manager` owns the `cdp` and then adds ETH into it
* `lockTokenCollateral(address mngr, address join, uint cdp, uint amt, uint transferFrom)` - locks tokens inside a CDP that was created with a `CDPManager`
* `safeLockTokenCollateral(address m, address join, uint cdp, uint a, uint t, address o)` - first checks to see if the manager \(`m`\) owns the `cdp` and then locks tokens in it
* `freeETH(address manager, address ethJoin, uint cdp, uint wad)` - withdraw ETH from a `cdp` created with `manager`
* `freeTokenCollateral(address manager, address join, uint cdp, uint amt)` - withdraw token collateral from a `cdp` created with `manager`
* `exitETH(address manager, address ethJoin, uint cdp, uint wad)` - exit ETH from `CDPEngine` and send it to the user \(through their own proxy\)
* `exitTokenCollateral(address manager, address collateralJoin, uint cdp, uint amt)` - exit token collateral from `CDPEngine` and send it to the user \(through their own proxy\)
* `generateDebt(address manager, address tax, address coinJoin, uint cdp, uint wad)` - generate `wad` amount of debt with `cdp` and exit the coins \(into their ERC20 form\) from `CDPEngine` to the user's address 
* `repayDebt(address manager, address coinJoin, uint cdp, uint wad)` - repay some of the `cdp`'s debt \(the CDP being owned by `manager` and controlled by the user through their own proxy\)
* `safeRepayDebt(address manager, address coinJoin, uint cdp, uint wad, address owner)` - first checks if the cdp is owned by `owner` \(inside `manager`\) and then repays some of its debt
* `repayAllDebt(address manager, address coinJoin, uint cdp)` - repay all the `cdp`'s debt \(the CDP being owned by `manager` and controlled by the user through their own proxy\)
* `safeRepayAllDebt(address manager, address coinJoin, uint cdp, address owner)` - first check if the cdp is owned by `owner` \(inside `manager`\) and then repay all of its debt
* `lockETHAndGenerateDebt()` - lock ETH inside a CDP \(owned by a `CDPManager` and controlled by the user through their own proxy\) and generate debt. The generated debt is `exit`ed from the `CDPEngine` in the form of ERC20 tokens and then sent to the user
* `openLockETHAndGenerateDebt()` - open a new CDP inside a `CDPManager`, lock ETH inside it and generate debt. The generated debt is `exit`ed from the `CDPEngine` in the form of ERC20 tokens and then sent to the user
* `lockTokenCollateralAndGenerateDebt()` - lock token collateral inside a CDP \(owned by a `CDPManager` and controlled by the user through their own proxy\) and generate debt. The generated debt is `exit`ed from the `CDPEngine` in the form of ERC20 tokens and then sent to the user
* `openLockTokenCollateralAndGenerateDebt()` - open a new CDP inside a `CDPManager`, lock token collateral inside it and generate debt. The generated debt is `exit`ed from the `CDPEngine` in the form of ERC20 tokens and then sent to the user
* `openLockGNTAndGenerateDebt()` - create a bag, transfer GNT to it, open a new CDP inside a `CDPManager`, lock the GNT inside it and generate debt. The generated debt is `exit`ed from the `CDPEngine` in the form of ERC20 tokens and then sent to the user
* `repayDebtAndFreeETH()` - repay some of the debt in a CDP owned by a `CDPManager` and controlled by the user \(using their own proxy\). After repayment, it exits a portion of ETH from the CDP and sends the ETH to the user's wallet
* `repayAllDebtAndFreeETH()` - similar to `repayDebtAndFreeETH` although it repays all the CDP's debt and sends all ETH to the user's wallet
* `repayDebtAndFreeTokenCollateral()` - repay some of the debt in a CDP owned by a `CDPManager` and controlled by the user \(using their own proxy\). After repayment, it exits a portion of ETH from the CDP and sends the ETH to the user's wallet
* `repayAllDebtAndFreeTokenCollateral()` - similar to `repayDebtAndFreeTokenCollateral` although it repays all the CDP's debt and sends all token collateral to the user's wallet



