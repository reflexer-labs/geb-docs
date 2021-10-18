# Proxy Actions

Convenience class to call functions from [GebProxyActions](https://github.com/reflexer-labs/geb-proxy-actions/blob/master/src/GebProxyActions.sol) through a proxy contract registered in the [GebProxyRegistry](https://github.com/reflexer-labs/geb-proxy-registry/blob/master/src/GebProxyRegistry.sol). These actions bundle multiple actions in one (e.g: open a safe + lock some ETH + draw some system coins).

## Constructors

\+ **new GebProxyActions**(`proxyAddress`: string, `network`: GebDeployment, `chainProvider`: GebProviderInterface): [_GebProxyActions_](gebproxyactions.md)

_Defined in _[_packages/geb/src/proxy-action.ts:57_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L57)

**Parameters:**

| Name            | Type                 |
| --------------- | -------------------- |
| `proxyAddress`  | string               |
| `network`       | GebDeployment        |
| `chainProvider` | GebProviderInterface |

**Returns:** [_GebProxyActions_](gebproxyactions.md)

## Properties

### proxy

• **proxy**: _DsProxy_

_Defined in _[_packages/geb/src/proxy-action.ts:28_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L28)

Underlying proxy object. Can be used to make custom calls to the proxy using the `proxy.execute()` function.

### proxyActionCoreAddress

• **proxyActionCoreAddress**: _string_

_Defined in _[_packages/geb/src/proxy-action.ts:33_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L33)

Address of the base proxy action contract.

### proxyActionGlobalSettlementAddress

• **proxyActionGlobalSettlementAddress**: _string_

_Defined in _[_packages/geb/src/proxy-action.ts:38_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L38)

Address of the proxy action contract for global settlement.

### proxyActionIncentiveAddress

• **proxyActionIncentiveAddress**: _string_

_Defined in _[_packages/geb/src/proxy-action.ts:43_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L43)

Address of the proxy action contract for Uniswap LP share staking.

### proxyActionLeverageAddress

• **proxyActionLeverageAddress**: _string_

_Defined in _[_packages/geb/src/proxy-action.ts:48_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L48)

Address of the proxy action contract used for leverage with flash loans.

### proxyAddress

• **proxyAddress**: _string_

_Defined in _[_packages/geb/src/proxy-action.ts:62_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L62)

Address of the underlying proxy.

## Methods

### allowSAFE

▸ **allowSAFE**(`safe`: BigNumberish, `usr`: string, `ok`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:115_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L115)

**Parameters:**

| Name   | Type         |
| ------ | ------------ |
| `safe` | BigNumberish |
| `usr`  | string       |
| `ok`   | BigNumberish |

**Returns:** _TransactionRequest_

### approveSAFEModification

▸ **approveSAFEModification**(`obj`: string, `usr`: string): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:130_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L130)

**Parameters:**

| Name  | Type   |
| ----- | ------ |
| `obj` | string |
| `usr` | string |

**Returns:** _TransactionRequest_

### coinJoin\_join

▸ **coinJoin\_join**(`apt`: string, `safeHandler`: string, `wad`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:136_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L136)

**Parameters:**

| Name          | Type         |
| ------------- | ------------ |
| `apt`         | string       |
| `safeHandler` | string       |
| `wad`         | BigNumberish |

**Returns:** _TransactionRequest_

### denySAFEModification

▸ **denySAFEModification**(`obj`: string, `usr`: string): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:146_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L146)

**Parameters:**

| Name  | Type   |
| ----- | ------ |
| `obj` | string |
| `usr` | string |

**Returns:** _TransactionRequest_

### enterSystem

▸ **enterSystem**(`src`: string, `safe`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:152_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L152)

**Parameters:**

| Name   | Type         |
| ------ | ------------ |
| `src`  | string       |
| `safe` | BigNumberish |

**Returns:** _TransactionRequest_

### exitETH

▸ **exitETH**(`safe`: BigNumberish, `wad`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:162_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L162)

**Parameters:**

| Name   | Type         |
| ------ | ------------ |
| `safe` | BigNumberish |
| `wad`  | BigNumberish |

**Returns:** _TransactionRequest_

### exitTokenCollateral

▸ **exitTokenCollateral**(`collateralJoin`: string, `safe`: BigNumberish, `amt`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:173_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L173)

**Parameters:**

| Name             | Type         |
| ---------------- | ------------ |
| `collateralJoin` | string       |
| `safe`           | BigNumberish |
| `amt`            | BigNumberish |

**Returns:** _TransactionRequest_

### flashDeleverage

▸ **flashDeleverage**(`uniswapV2Pair`: string, `callbackProxy`: string, `collateralType`: BytesLike, `safe`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:815_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L815)

**Parameters:**

| Name             | Type         |
| ---------------- | ------------ |
| `uniswapV2Pair`  | string       |
| `callbackProxy`  | string       |
| `collateralType` | BytesLike    |
| `safe`           | BigNumberish |

**Returns:** _TransactionRequest_

### flashDeleverageFreeETH

▸ **flashDeleverageFreeETH**(`uniswapV2Pair`: string, `callbackProxy`: string, `collateralType`: BytesLike, `safe`: BigNumberish, `amountToFree`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:836_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L836)

**Parameters:**

| Name             | Type         |
| ---------------- | ------------ |
| `uniswapV2Pair`  | string       |
| `callbackProxy`  | string       |
| `collateralType` | BytesLike    |
| `safe`           | BigNumberish |
| `amountToFree`   | BigNumberish |

**Returns:** _TransactionRequest_

### flashLeverage

▸ **flashLeverage**(`uniswapV2Pair`: string, `callbackProxy`: string, `collateralType`: BytesLike, `safe`: BigNumberish, `leverage`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:859_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L859)

**Parameters:**

| Name             | Type         |
| ---------------- | ------------ |
| `uniswapV2Pair`  | string       |
| `callbackProxy`  | string       |
| `collateralType` | BytesLike    |
| `safe`           | BigNumberish |
| `leverage`       | BigNumberish |

**Returns:** _TransactionRequest_

### freeETH

▸ **freeETH**(`safe`: BigNumberish, `wad`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:188_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L188)

**Parameters:**

| Name   | Type         |
| ------ | ------------ |
| `safe` | BigNumberish |
| `wad`  | BigNumberish |

**Returns:** _TransactionRequest_

### freeTokenCollateral

▸ **freeTokenCollateral**(`collateralJoin`: string, `safe`: BigNumberish, `amt`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:199_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L199)

**Parameters:**

| Name             | Type         |
| ---------------- | ------------ |
| `collateralJoin` | string       |
| `safe`           | BigNumberish |
| `amt`            | BigNumberish |

**Returns:** _TransactionRequest_

### freeTokenCollateralGlobalSettlement

▸ **freeTokenCollateralGlobalSettlement**(`collateralJoin`: string, `safe`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:765_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L765)

**Parameters:**

| Name             | Type         |
| ---------------- | ------------ |
| `collateralJoin` | string       |
| `safe`           | BigNumberish |

**Returns:** _TransactionRequest_

### generateDebt

▸ **generateDebt**(`safe`: BigNumberish, `wad`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:214_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L214)

**Parameters:**

| Name   | Type         |
| ------ | ------------ |
| `safe` | BigNumberish |
| `wad`  | BigNumberish |

**Returns:** _TransactionRequest_

### generateDebtAndProtectSAFE

▸ **generateDebtAndProtectSAFE**(`safe`: BigNumberish, `wad`: BigNumberish, `saviour`: string): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:226_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L226)

**Parameters:**

| Name      | Type         |
| --------- | ------------ |
| `safe`    | BigNumberish |
| `wad`     | BigNumberish |
| `saviour` | string       |

**Returns:** _TransactionRequest_

### lockETH

▸ **lockETH**(`ethValue`: BigNumberish, `safe`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:244_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L244)

**Parameters:**

| Name       | Type         |
| ---------- | ------------ |
| `ethValue` | BigNumberish |
| `safe`     | BigNumberish |

**Returns:** _TransactionRequest_

### lockETHAndGenerateDebt

▸ **lockETHAndGenerateDebt**(`ethValue`: BigNumberish, `safe`: BigNumberish, `deltaWad`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:255_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L255)

**Parameters:**

| Name       | Type         |
| ---------- | ------------ |
| `ethValue` | BigNumberish |
| `safe`     | BigNumberish |
| `deltaWad` | BigNumberish |

**Returns:** _TransactionRequest_

### lockETHLeverage

▸ **lockETHLeverage**(`ethValue`: BigNumberish, `uniswapV2Pair`: string, `callbackProxy`: string, `collateralType`: BytesLike, `safe`: BigNumberish, `leverage`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:882_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L882)

**Parameters:**

| Name             | Type         |
| ---------------- | ------------ |
| `ethValue`       | BigNumberish |
| `uniswapV2Pair`  | string       |
| `callbackProxy`  | string       |
| `collateralType` | BytesLike    |
| `safe`           | BigNumberish |
| `leverage`       | BigNumberish |

**Returns:** _TransactionRequest_

### lockTokenCollateral

▸ **lockTokenCollateral**(`collateralJoin`: string, `safe`: BigNumberish, `amt`: BigNumberish, `transferFrom`: boolean): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:273_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L273)

**Parameters:**

| Name             | Type         |
| ---------------- | ------------ |
| `collateralJoin` | string       |
| `safe`           | BigNumberish |
| `amt`            | BigNumberish |
| `transferFrom`   | boolean      |

**Returns:** _TransactionRequest_

### lockTokenCollateralAndGenerateDebt

▸ **lockTokenCollateralAndGenerateDebt**(`collateralJoin`: string, `safe`: BigNumberish, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish, `transferFrom`: boolean): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:290_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L290)

**Parameters:**

| Name               | Type         |
| ------------------ | ------------ |
| `collateralJoin`   | string       |
| `safe`             | BigNumberish |
| `collateralAmount` | BigNumberish |
| `deltaWad`         | BigNumberish |
| `transferFrom`     | boolean      |

**Returns:** _TransactionRequest_

### lockTokenCollateralGenerateDebtAndProtectSAFE

▸ **lockTokenCollateralGenerateDebtAndProtectSAFE**(`collateralJoin`: string, `safe`: BigNumberish, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish, `transferFrom`: boolean, `saviour`: string): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:311_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L311)

**Parameters:**

| Name               | Type         |
| ------------------ | ------------ |
| `collateralJoin`   | string       |
| `safe`             | BigNumberish |
| `collateralAmount` | BigNumberish |
| `deltaWad`         | BigNumberish |
| `transferFrom`     | boolean      |
| `saviour`          | string       |

**Returns:** _TransactionRequest_

### makeCollateralBag

▸ **makeCollateralBag**(`collateralJoin`: string): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:335_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L335)

**Parameters:**

| Name             | Type   |
| ---------------- | ------ |
| `collateralJoin` | string |

**Returns:** _TransactionRequest_

### modifySAFECollateralization

▸ **modifySAFECollateralization**(`safe`: BigNumberish, `deltaCollateral`: BigNumberish, `deltaDebt`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:341_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L341)

**Parameters:**

| Name              | Type         |
| ----------------- | ------------ |
| `safe`            | BigNumberish |
| `deltaCollateral` | BigNumberish |
| `deltaDebt`       | BigNumberish |

**Returns:** _TransactionRequest_

### moveSAFE

▸ **moveSAFE**(`safeSrc`: BigNumberish, `safeDst`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:356_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L356)

**Parameters:**

| Name      | Type         |
| --------- | ------------ |
| `safeSrc` | BigNumberish |
| `safeDst` | BigNumberish |

**Returns:** _TransactionRequest_

### openLockETHAndGenerateDebt

▸ **openLockETHAndGenerateDebt**(`ethValue`: BigNumberish, `collateralType`: BytesLike, `deltaWad`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:366_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L366)

**Parameters:**

| Name             | Type         |
| ---------------- | ------------ |
| `ethValue`       | BigNumberish |
| `collateralType` | BytesLike    |
| `deltaWad`       | BigNumberish |

**Returns:** _TransactionRequest_

### openLockETHGenerateDebtAndProtectSAFE

▸ **openLockETHGenerateDebtAndProtectSAFE**(`ethValue`: BigNumberish, `collateralType`: BytesLike, `deltaWad`: BigNumberish, `saviour`: string): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:384_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L384)

**Parameters:**

| Name             | Type         |
| ---------------- | ------------ |
| `ethValue`       | BigNumberish |
| `collateralType` | BytesLike    |
| `deltaWad`       | BigNumberish |
| `saviour`        | string       |

**Returns:** _TransactionRequest_

### openLockETHLeverage

▸ **openLockETHLeverage**(`ethValue`: BigNumberish, `uniswapV2Pair`: string, `callbackProxy`: string, `collateralType`: BytesLike, `leverage`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:907_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L907)

**Parameters:**

| Name             | Type         |
| ---------------- | ------------ |
| `ethValue`       | BigNumberish |
| `uniswapV2Pair`  | string       |
| `callbackProxy`  | string       |
| `collateralType` | BytesLike    |
| `leverage`       | BigNumberish |

**Returns:** _TransactionRequest_

### openLockGNTAndGenerateDebt

▸ **openLockGNTAndGenerateDebt**(`gntJoin`: string, `collateralType`: BytesLike, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:405_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L405)

**Parameters:**

| Name               | Type         |
| ------------------ | ------------ |
| `gntJoin`          | string       |
| `collateralType`   | BytesLike    |
| `collateralAmount` | BigNumberish |
| `deltaWad`         | BigNumberish |

**Returns:** _TransactionRequest_

### openLockGNTGenerateDebtAndProtectSAFE

▸ **openLockGNTGenerateDebtAndProtectSAFE**(`gntJoin`: string, `collateralType`: BytesLike, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish, `saviour`: string): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:424_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L424)

**Parameters:**

| Name               | Type         |
| ------------------ | ------------ |
| `gntJoin`          | string       |
| `collateralType`   | BytesLike    |
| `collateralAmount` | BigNumberish |
| `deltaWad`         | BigNumberish |
| `saviour`          | string       |

**Returns:** _TransactionRequest_

### openLockTokenCollateralAndGenerateDebt

▸ **openLockTokenCollateralAndGenerateDebt**(`collateralJoin`: string, `collateralType`: BytesLike, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish, `transferFrom`: boolean): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:446_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L446)

**Parameters:**

| Name               | Type         |
| ------------------ | ------------ |
| `collateralJoin`   | string       |
| `collateralType`   | BytesLike    |
| `collateralAmount` | BigNumberish |
| `deltaWad`         | BigNumberish |
| `transferFrom`     | boolean      |

**Returns:** _TransactionRequest_

### openLockTokenCollateralGenerateDebtAndProtectSAFE

▸ **openLockTokenCollateralGenerateDebtAndProtectSAFE**(`collateralJoin`: string, `collateralType`: BytesLike, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish, `transferFrom`: boolean, `saviour`: string): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:467_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L467)

**Parameters:**

| Name               | Type         |
| ------------------ | ------------ |
| `collateralJoin`   | string       |
| `collateralType`   | BytesLike    |
| `collateralAmount` | BigNumberish |
| `deltaWad`         | BigNumberish |
| `transferFrom`     | boolean      |
| `saviour`          | string       |

**Returns:** _TransactionRequest_

### openSAFE

▸ **openSAFE**(`collateralType`: BytesLike, `usr`: string): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:491_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L491)

**Parameters:**

| Name             | Type      |
| ---------------- | --------- |
| `collateralType` | BytesLike |
| `usr`            | string    |

**Returns:** _TransactionRequest_

### prepareCoinsForRedeemingGlobalSettlement

▸ **prepareCoinsForRedeemingGlobalSettlement**(`wad`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:753_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L753)

**Parameters:**

| Name  | Type         |
| ----- | ------------ |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

### protectSAFE

▸ **protectSAFE**(`safe`: BigNumberish, `saviour`: string): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:501_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L501)

**Parameters:**

| Name      | Type         |
| --------- | ------------ |
| `safe`    | BigNumberish |
| `saviour` | string       |

**Returns:** _TransactionRequest_

### quitSystem

▸ **quitSystem**(`safe`: BigNumberish, `dst`: string): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:512_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L512)

**Parameters:**

| Name   | Type         |
| ------ | ------------ |
| `safe` | BigNumberish |
| `dst`  | string       |

**Returns:** _TransactionRequest_

### redeemETHGlobalSettlement

▸ **redeemETHGlobalSettlement**(`ethJoin`: string, `collateralType`: BytesLike, `wad`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:779_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L779)

**Parameters:**

| Name             | Type         |
| ---------------- | ------------ |
| `ethJoin`        | string       |
| `collateralType` | BytesLike    |
| `wad`            | BigNumberish |

**Returns:** _TransactionRequest_

### redeemTokenCollateralGlobalSettlement

▸ **redeemTokenCollateralGlobalSettlement**(`collateralJoin`: string, `collateralType`: BytesLike, `wad`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:794_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L794)

**Parameters:**

| Name             | Type         |
| ---------------- | ------------ |
| `collateralJoin` | string       |
| `collateralType` | BytesLike    |
| `wad`            | BigNumberish |

**Returns:** _TransactionRequest_

### repayAllDebt

▸ **repayAllDebt**(`safe`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:522_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L522)

**Parameters:**

| Name   | Type         |
| ------ | ------------ |
| `safe` | BigNumberish |

**Returns:** _TransactionRequest_

### repayAllDebtAndFreeETH

▸ **repayAllDebtAndFreeETH**(`safe`: BigNumberish, `collateralWad`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:532_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L532)

**Parameters:**

| Name            | Type         |
| --------------- | ------------ |
| `safe`          | BigNumberish |
| `collateralWad` | BigNumberish |

**Returns:** _TransactionRequest_

### repayAllDebtAndFreeTokenCollateral

▸ **repayAllDebtAndFreeTokenCollateral**(`collateralJoin`: string, `safe`: BigNumberish, `collateralAmount`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:547_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L547)

**Parameters:**

| Name               | Type         |
| ------------------ | ------------ |
| `collateralJoin`   | string       |
| `safe`             | BigNumberish |
| `collateralAmount` | BigNumberish |

**Returns:** _TransactionRequest_

### repayDebt

▸ **repayDebt**(`safe`: BigNumberish, `wad`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:563_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L563)

**Parameters:**

| Name   | Type         |
| ------ | ------------ |
| `safe` | BigNumberish |
| `wad`  | BigNumberish |

**Returns:** _TransactionRequest_

### repayDebtAndFreeETH

▸ **repayDebtAndFreeETH**(`safe`: BigNumberish, `collateralWad`: BigNumberish, `deltaWad`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:574_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L574)

**Parameters:**

| Name            | Type         |
| --------------- | ------------ |
| `safe`          | BigNumberish |
| `collateralWad` | BigNumberish |
| `deltaWad`      | BigNumberish |

**Returns:** _TransactionRequest_

### repayDebtAndFreeTokenCollateral

▸ **repayDebtAndFreeTokenCollateral**(`collateralJoin`: string, `safe`: BigNumberish, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:591_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L591)

**Parameters:**

| Name               | Type         |
| ------------------ | ------------ |
| `collateralJoin`   | string       |
| `safe`             | BigNumberish |
| `collateralAmount` | BigNumberish |
| `deltaWad`         | BigNumberish |

**Returns:** _TransactionRequest_

### safeLockETH

▸ **safeLockETH**(`ethValue`: BigNumberish, `safe`: BigNumberish, `owner`: string): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:609_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L609)

**Parameters:**

| Name       | Type         |
| ---------- | ------------ |
| `ethValue` | BigNumberish |
| `safe`     | BigNumberish |
| `owner`    | string       |

**Returns:** _TransactionRequest_

### safeLockTokenCollateral

▸ **safeLockTokenCollateral**(`collateralJoin`: string, `safe`: BigNumberish, `amt`: BigNumberish, `transferFrom`: boolean, `owner`: string): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:625_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L625)

**Parameters:**

| Name             | Type         |
| ---------------- | ------------ |
| `collateralJoin` | string       |
| `safe`           | BigNumberish |
| `amt`            | BigNumberish |
| `transferFrom`   | boolean      |
| `owner`          | string       |

**Returns:** _TransactionRequest_

### safeRepayAllDebt

▸ **safeRepayAllDebt**(`safe`: BigNumberish, `owner`: string): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:644_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L644)

**Parameters:**

| Name    | Type         |
| ------- | ------------ |
| `safe`  | BigNumberish |
| `owner` | string       |

**Returns:** _TransactionRequest_

### safeRepayDebt

▸ **safeRepayDebt**(`safe`: BigNumberish, `wad`: BigNumberish, `owner`: string): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:655_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L655)

**Parameters:**

| Name    | Type         |
| ------- | ------------ |
| `safe`  | BigNumberish |
| `wad`   | BigNumberish |
| `owner` | string       |

**Returns:** _TransactionRequest_

### tokenCollateralJoin\_join

▸ **tokenCollateralJoin\_join**(`apt`: string, `safe`: string, `amt`: BigNumberish, `transferFrom`: boolean): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:671_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L671)

**Parameters:**

| Name           | Type         |
| -------------- | ------------ |
| `apt`          | string       |
| `safe`         | string       |
| `amt`          | BigNumberish |
| `transferFrom` | boolean      |

**Returns:** _TransactionRequest_

### transfer

▸ **transfer**(`collateral`: string, `dst`: string, `amt`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:687_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L687)

**Parameters:**

| Name         | Type         |
| ------------ | ------------ |
| `collateral` | string       |
| `dst`        | string       |
| `amt`        | BigNumberish |

**Returns:** _TransactionRequest_

### transferCollateral

▸ **transferCollateral**(`safe`: BigNumberish, `dst`: string, `wad`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:697_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L697)

**Parameters:**

| Name   | Type         |
| ------ | ------------ |
| `safe` | BigNumberish |
| `dst`  | string       |
| `wad`  | BigNumberish |

**Returns:** _TransactionRequest_

### transferInternalCoins

▸ **transferInternalCoins**(`safe`: BigNumberish, `dst`: string, `rad`: BigNumberish): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:712_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L712)

**Parameters:**

| Name   | Type         |
| ------ | ------------ |
| `safe` | BigNumberish |
| `dst`  | string       |
| `rad`  | BigNumberish |

**Returns:** _TransactionRequest_

### transferSAFEOwnership

▸ **transferSAFEOwnership**(`safe`: BigNumberish, `usr`: string): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:727_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L727)

**Parameters:**

| Name   | Type         |
| ------ | ------------ |
| `safe` | BigNumberish |
| `usr`  | string       |

**Returns:** _TransactionRequest_

### transferSAFEOwnershipToProxy

▸ **transferSAFEOwnershipToProxy**(`safe`: BigNumberish, `dst`: string): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:737_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L737)

**Parameters:**

| Name   | Type         |
| ------ | ------------ |
| `safe` | BigNumberish |
| `dst`  | string       |

**Returns:** _TransactionRequest_

### uniswapV2Call

▸ **uniswapV2Call**(`_sender`: string, `_amount0`: BigNumberish, `_amount1`: BigNumberish, `_data`: BytesLike): _TransactionRequest_

_Defined in _[_packages/geb/src/proxy-action.ts:930_](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L930)

**Parameters:**

| Name       | Type         |
| ---------- | ------------ |
| `_sender`  | string       |
| `_amount0` | BigNumberish |
| `_amount1` | BigNumberish |
| `_data`    | BytesLike    |

**Returns:** _TransactionRequest_
