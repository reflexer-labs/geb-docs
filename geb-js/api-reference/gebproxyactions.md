# Proxy Actions

Convenience class to call functions from [GebProxyActions](https://github.com/reflexer-labs/geb-proxy-actions/blob/master/src/GebProxyActions.sol) through a proxy contract registered in the [GebProxyRegistry](https://github.com/reflexer-labs/geb-proxy-registry/blob/master/src/GebProxyRegistry.sol). These actions bundle multiple actions in one \(e.g: open a safe + lock some ETH + draw some RAI\).

## Constructors

+ **new GebProxyActions**\(`proxyAddress`: string, `network`: GebDeployment, `chainProvider`: GebProviderInterface\): [_GebProxyActions_](gebproxyactions.md)

_Defined in_ [_packages/geb/src/proxy-action.ts:55_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L55)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `proxyAddress` | string |
| `network` | GebDeployment |
| `chainProvider` | GebProviderInterface |

**Returns:** [_GebProxyActions_](gebproxyactions.md)

## Properties

### proxy

• **proxy**: _DsProxy_

_Defined in_ [_packages/geb/src/proxy-action.ts:29_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L29)

Underlying proxy object. Can be use to make custom calls to the proxy using `proxy.execute()` function. For the details of each function

### proxyActionCoreAddress

• **proxyActionCoreAddress**: _string_

_Defined in_ [_packages/geb/src/proxy-action.ts:34_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L34)

Address of the base proxy action contract.

### proxyActionGlobalSettlementAddress

• **proxyActionGlobalSettlementAddress**: _string_

_Defined in_ [_packages/geb/src/proxy-action.ts:39_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L39)

Address of the proxy action contract for global settlement.

### proxyActionIncentiveAddress

• **proxyActionIncentiveAddress**: _string_

_Defined in_ [_packages/geb/src/proxy-action.ts:44_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L44)

Address of the proxy action contract for uniswap LP share staking.

### proxyActionLeverageAddress

• **proxyActionLeverageAddress**: _string_

_Defined in_ [_packages/geb/src/proxy-action.ts:49_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L49)

Address of the proxy action contract for leveraged with flash loans operations.

### proxyAddress

• **proxyAddress**: _string_

_Defined in_ [_packages/geb/src/proxy-action.ts:60_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L60)

Address of the underlying proxy

## Methods

### allowHandler

▸ **allowHandler**\(`usr`: string, `ok`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:820_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L820)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `usr` | string |
| `ok` | BigNumberish |

**Returns:** _TransactionRequest_

### allowSAFE

▸ **allowSAFE**\(`safe`: BigNumberish, `usr`: string, `ok`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:110_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L110)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `usr` | string |
| `ok` | BigNumberish |

**Returns:** _TransactionRequest_

### approveSAFEModification

▸ **approveSAFEModification**\(`obj`: string, `usr`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:125_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L125)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `obj` | string |
| `usr` | string |

**Returns:** _TransactionRequest_

### coinJoin\_join

▸ **coinJoin\_join**\(`apt`: string, `safeHandler`: string, `wad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:131_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L131)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `apt` | string |
| `safeHandler` | string |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

### denySAFEModification

▸ **denySAFEModification**\(`obj`: string, `usr`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:141_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L141)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `obj` | string |
| `usr` | string |

**Returns:** _TransactionRequest_

### enterSystem

▸ **enterSystem**\(`src`: string, `safe`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:147_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L147)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `src` | string |
| `safe` | BigNumberish |

**Returns:** _TransactionRequest_

### ethJoin\_join

▸ **ethJoin\_join**\(`ethValue`: BigNumberish, `apt`: string, `safe`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:157_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L157)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `ethValue` | BigNumberish |
| `apt` | string |
| `safe` | string |

**Returns:** _TransactionRequest_

### exitAndRemoveLiquidity

▸ **exitAndRemoveLiquidity**\(`minTokenAmounts`: \[BigNumberish, BigNumberish\]\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:830_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L830)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `minTokenAmounts` | \[BigNumberish, BigNumberish\] |

**Returns:** _TransactionRequest_

### exitETH

▸ **exitETH**\(`safe`: BigNumberish, `wad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:171_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L171)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

### exitMine

▸ **exitMine**\(`incentives`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:843_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L843)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `incentives` | string |

**Returns:** _TransactionRequest_

### exitRemoveLiquidityRepayDebt

▸ **exitRemoveLiquidityRepayDebt**\(`safe`: BigNumberish, `minTokenAmounts`: \[BigNumberish, BigNumberish\]\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:849_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L849)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `minTokenAmounts` | \[BigNumberish, BigNumberish\] |

**Returns:** _TransactionRequest_

### exitRemoveLiquidityRepayDebtFreeETH

▸ **exitRemoveLiquidityRepayDebtFreeETH**\(`safe`: BigNumberish, `ethToFree`: BigNumberish, `minTokenAmounts`: \[BigNumberish, BigNumberish\]\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:865_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L865)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `ethToFree` | BigNumberish |
| `minTokenAmounts` | \[BigNumberish, BigNumberish\] |

**Returns:** _TransactionRequest_

### exitTokenCollateral

▸ **exitTokenCollateral**\(`collateralJoin`: string, `safe`: BigNumberish, `amt`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:182_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L182)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `collateralJoin` | string |
| `safe` | BigNumberish |
| `amt` | BigNumberish |

**Returns:** _TransactionRequest_

### flashDeleverage

▸ **flashDeleverage**\(`uniswapV2Pair`: string, `callbackProxy`: string, `safe`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:1151_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1151)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `uniswapV2Pair` | string |
| `callbackProxy` | string |
| `safe` | BigNumberish |

**Returns:** _TransactionRequest_

### flashDeleverageFreeETH

▸ **flashDeleverageFreeETH**\(`uniswapV2Pair`: string, `callbackProxy`: string, `safe`: BigNumberish, `amountToFree`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:1170_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1170)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `uniswapV2Pair` | string |
| `callbackProxy` | string |
| `safe` | BigNumberish |
| `amountToFree` | BigNumberish |

**Returns:** _TransactionRequest_

### flashLeverage

▸ **flashLeverage**\(`uniswapV2Pair`: string, `callbackProxy`: string, `safe`: BigNumberish, `leverage`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:1191_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1191)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `uniswapV2Pair` | string |
| `callbackProxy` | string |
| `safe` | BigNumberish |
| `leverage` | BigNumberish |

**Returns:** _TransactionRequest_

### freeETH

▸ **freeETH**\(`safe`: BigNumberish, `wad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:197_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L197)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

### freeTokenCollateral

▸ **freeTokenCollateral**\(`collateralJoin`: string, `safe`: BigNumberish, `amt`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:208_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L208)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `collateralJoin` | string |
| `safe` | BigNumberish |
| `amt` | BigNumberish |

**Returns:** _TransactionRequest_

### freeTokenCollateralGlobalSettlement

▸ **freeTokenCollateralGlobalSettlement**\(`collateralJoin`: string, `safe`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:774_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L774)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `collateralJoin` | string |
| `safe` | BigNumberish |

**Returns:** _TransactionRequest_

### generateDebt

▸ **generateDebt**\(`safe`: BigNumberish, `wad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:223_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L223)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

### generateDebtAndProtectSAFE

▸ **generateDebtAndProtectSAFE**\(`safe`: BigNumberish, `wad`: BigNumberish, `saviour`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:235_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L235)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `wad` | BigNumberish |
| `saviour` | string |

**Returns:** _TransactionRequest_

### generateDebtAndProvideLiquidityStake

▸ **generateDebtAndProvideLiquidityStake**\(`ethValue`: BigNumberish, `safe`: BigNumberish, `wad`: BigNumberish, `minTokenAmounts`: \[BigNumberish, BigNumberish\]\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:884_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L884)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `ethValue` | BigNumberish |
| `safe` | BigNumberish |
| `wad` | BigNumberish |
| `minTokenAmounts` | \[BigNumberish, BigNumberish\] |

**Returns:** _TransactionRequest_

### generateDebtAndProvideLiquidityUniswap

▸ **generateDebtAndProvideLiquidityUniswap**\(`ethValue`: BigNumberish, `safe`: BigNumberish, `wad`: BigNumberish, `minTokenAmounts`: \[BigNumberish, BigNumberish\]\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:905_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L905)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `ethValue` | BigNumberish |
| `safe` | BigNumberish |
| `wad` | BigNumberish |
| `minTokenAmounts` | \[BigNumberish, BigNumberish\] |

**Returns:** _TransactionRequest_

### getLockedReward

▸ **getLockedReward**\(`campaignId`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:925_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L925)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `campaignId` | BigNumberish |

**Returns:** _TransactionRequest_

### harvestReward

▸ **harvestReward**\(`campaignId`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:934_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L934)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `campaignId` | BigNumberish |

**Returns:** _TransactionRequest_

### lockETH

▸ **lockETH**\(`ethValue`: BigNumberish, `safe`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:253_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L253)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `ethValue` | BigNumberish |
| `safe` | BigNumberish |

**Returns:** _TransactionRequest_

### lockETHAndGenerateDebt

▸ **lockETHAndGenerateDebt**\(`ethValue`: BigNumberish, `safe`: BigNumberish, `deltaWad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:264_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L264)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `ethValue` | BigNumberish |
| `safe` | BigNumberish |
| `deltaWad` | BigNumberish |

**Returns:** _TransactionRequest_

### lockETHGenerateDebtProvideLiquidityStake

▸ **lockETHGenerateDebtProvideLiquidityStake**\(`ethValue`: BigNumberish, `safe`: BigNumberish, `deltaWad`: BigNumberish, `liquidityWad`: BigNumberish, `minTokenAmounts`: \[BigNumberish, BigNumberish\]\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:943_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L943)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `ethValue` | BigNumberish |
| `safe` | BigNumberish |
| `deltaWad` | BigNumberish |
| `liquidityWad` | BigNumberish |
| `minTokenAmounts` | \[BigNumberish, BigNumberish\] |

**Returns:** _TransactionRequest_

### lockETHGenerateDebtProvideLiquidityUniswap

▸ **lockETHGenerateDebtProvideLiquidityUniswap**\(`ethValue`: BigNumberish, `safe`: BigNumberish, `deltaWad`: BigNumberish, `liquidityWad`: BigNumberish, `minTokenAmounts`: \[BigNumberish, BigNumberish\]\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:967_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L967)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `ethValue` | BigNumberish |
| `safe` | BigNumberish |
| `deltaWad` | BigNumberish |
| `liquidityWad` | BigNumberish |
| `minTokenAmounts` | \[BigNumberish, BigNumberish\] |

**Returns:** _TransactionRequest_

### lockETHLeverage

▸ **lockETHLeverage**\(`ethValue`: BigNumberish, `uniswapV2Pair`: string, `callbackProxy`: string, `safe`: BigNumberish, `leverage`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:1212_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1212)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `ethValue` | BigNumberish |
| `uniswapV2Pair` | string |
| `callbackProxy` | string |
| `safe` | BigNumberish |
| `leverage` | BigNumberish |

**Returns:** _TransactionRequest_

### lockTokenCollateral

▸ **lockTokenCollateral**\(`collateralJoin`: string, `safe`: BigNumberish, `amt`: BigNumberish, `transferFrom`: boolean\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:282_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L282)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `collateralJoin` | string |
| `safe` | BigNumberish |
| `amt` | BigNumberish |
| `transferFrom` | boolean |

**Returns:** _TransactionRequest_

### lockTokenCollateralAndGenerateDebt

▸ **lockTokenCollateralAndGenerateDebt**\(`collateralJoin`: string, `safe`: BigNumberish, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish, `transferFrom`: boolean\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:299_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L299)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `collateralJoin` | string |
| `safe` | BigNumberish |
| `collateralAmount` | BigNumberish |
| `deltaWad` | BigNumberish |
| `transferFrom` | boolean |

**Returns:** _TransactionRequest_

### lockTokenCollateralGenerateDebtAndProtectSAFE

▸ **lockTokenCollateralGenerateDebtAndProtectSAFE**\(`collateralJoin`: string, `safe`: BigNumberish, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish, `transferFrom`: boolean, `saviour`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:320_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L320)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `collateralJoin` | string |
| `safe` | BigNumberish |
| `collateralAmount` | BigNumberish |
| `deltaWad` | BigNumberish |
| `transferFrom` | boolean |
| `saviour` | string |

**Returns:** _TransactionRequest_

### makeCollateralBag

▸ **makeCollateralBag**\(`collateralJoin`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:344_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L344)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `collateralJoin` | string |

**Returns:** _TransactionRequest_

### modifySAFECollateralization

▸ **modifySAFECollateralization**\(`safe`: BigNumberish, `deltaCollateral`: BigNumberish, `deltaDebt`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:350_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L350)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `deltaCollateral` | BigNumberish |
| `deltaDebt` | BigNumberish |

**Returns:** _TransactionRequest_

### moveSAFE

▸ **moveSAFE**\(`safeSrc`: BigNumberish, `safeDst`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:365_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L365)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safeSrc` | BigNumberish |
| `safeDst` | BigNumberish |

**Returns:** _TransactionRequest_

### openLockETHAndGenerateDebt

▸ **openLockETHAndGenerateDebt**\(`ethValue`: BigNumberish, `collateralType`: BytesLike, `deltaWad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:375_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L375)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `ethValue` | BigNumberish |
| `collateralType` | BytesLike |
| `deltaWad` | BigNumberish |

**Returns:** _TransactionRequest_

### openLockETHGenerateDebtAndProtectSAFE

▸ **openLockETHGenerateDebtAndProtectSAFE**\(`ethValue`: BigNumberish, `collateralType`: BytesLike, `deltaWad`: BigNumberish, `saviour`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:393_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L393)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `ethValue` | BigNumberish |
| `collateralType` | BytesLike |
| `deltaWad` | BigNumberish |
| `saviour` | string |

**Returns:** _TransactionRequest_

### openLockETHGenerateDebtProvideLiquidityStake

▸ **openLockETHGenerateDebtProvideLiquidityStake**\(`ethValue`: BigNumberish, `deltaWad`: BigNumberish, `liquidityWad`: BigNumberish, `minTokenAmounts`: \[BigNumberish, BigNumberish\]\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:990_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L990)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `ethValue` | BigNumberish |
| `deltaWad` | BigNumberish |
| `liquidityWad` | BigNumberish |
| `minTokenAmounts` | \[BigNumberish, BigNumberish\] |

**Returns:** _TransactionRequest_

### openLockETHGenerateDebtProvideLiquidityUniswap

▸ **openLockETHGenerateDebtProvideLiquidityUniswap**\(`ethValue`: BigNumberish, `deltaWad`: BigNumberish, `liquidityWad`: BigNumberish, `minTokenAmounts`: \[BigNumberish, BigNumberish\]\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:1012_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1012)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `ethValue` | BigNumberish |
| `deltaWad` | BigNumberish |
| `liquidityWad` | BigNumberish |
| `minTokenAmounts` | \[BigNumberish, BigNumberish\] |

**Returns:** _TransactionRequest_

### openLockETHLeverage

▸ **openLockETHLeverage**\(`ethValue`: BigNumberish, `uniswapV2Pair`: string, `callbackProxy`: string, `leverage`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:1235_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1235)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `ethValue` | BigNumberish |
| `uniswapV2Pair` | string |
| `callbackProxy` | string |
| `leverage` | BigNumberish |

**Returns:** _TransactionRequest_

### openLockGNTAndGenerateDebt

▸ **openLockGNTAndGenerateDebt**\(`gntJoin`: string, `collateralType`: BytesLike, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:414_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L414)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `gntJoin` | string |
| `collateralType` | BytesLike |
| `collateralAmount` | BigNumberish |
| `deltaWad` | BigNumberish |

**Returns:** _TransactionRequest_

### openLockGNTGenerateDebtAndProtectSAFE

▸ **openLockGNTGenerateDebtAndProtectSAFE**\(`gntJoin`: string, `collateralType`: BytesLike, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish, `saviour`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:433_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L433)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `gntJoin` | string |
| `collateralType` | BytesLike |
| `collateralAmount` | BigNumberish |
| `deltaWad` | BigNumberish |
| `saviour` | string |

**Returns:** _TransactionRequest_

### openLockTokenCollateralAndGenerateDebt

▸ **openLockTokenCollateralAndGenerateDebt**\(`collateralJoin`: string, `collateralType`: BytesLike, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish, `transferFrom`: boolean\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:455_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L455)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `collateralJoin` | string |
| `collateralType` | BytesLike |
| `collateralAmount` | BigNumberish |
| `deltaWad` | BigNumberish |
| `transferFrom` | boolean |

**Returns:** _TransactionRequest_

### openLockTokenCollateralGenerateDebtAndProtectSAFE

▸ **openLockTokenCollateralGenerateDebtAndProtectSAFE**\(`collateralJoin`: string, `collateralType`: BytesLike, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish, `transferFrom`: boolean, `saviour`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:476_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L476)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `collateralJoin` | string |
| `collateralType` | BytesLike |
| `collateralAmount` | BigNumberish |
| `deltaWad` | BigNumberish |
| `transferFrom` | boolean |
| `saviour` | string |

**Returns:** _TransactionRequest_

### openSAFE

▸ **openSAFE**\(`collateralType`: BytesLike, `usr`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:500_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L500)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `collateralType` | BytesLike |
| `usr` | string |

**Returns:** _TransactionRequest_

### prepareCoinsForRedeemingGlobalSettlement

▸ **prepareCoinsForRedeemingGlobalSettlement**\(`wad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:762_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L762)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

### protectSAFE

▸ **protectSAFE**\(`safe`: BigNumberish, `saviour`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:510_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L510)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `saviour` | string |

**Returns:** _TransactionRequest_

### provideLiquidityUniswap

▸ **provideLiquidityUniswap**\(`ethValue`: BigNumberish, `wad`: BigNumberish, `minTokenAmounts`: \[BigNumberish, BigNumberish\]\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:1033_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1033)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `ethValue` | BigNumberish |
| `wad` | BigNumberish |
| `minTokenAmounts` | \[BigNumberish, BigNumberish\] |

**Returns:** _TransactionRequest_

### quitSystem

▸ **quitSystem**\(`safe`: BigNumberish, `dst`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:521_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L521)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `dst` | string |

**Returns:** _TransactionRequest_

### redeemETHGlobalSettlement

▸ **redeemETHGlobalSettlement**\(`ethJoin`: string, `collateralType`: BytesLike, `wad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:788_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L788)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `ethJoin` | string |
| `collateralType` | BytesLike |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

### redeemTokenCollateralGlobalSettlement

▸ **redeemTokenCollateralGlobalSettlement**\(`collateralJoin`: string, `collateralType`: BytesLike, `wad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:803_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L803)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `collateralJoin` | string |
| `collateralType` | BytesLike |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

### removeLiquidityUniswap

▸ **removeLiquidityUniswap**\(`systemCoin`: string, `value`: BigNumberish, `minTokenAmounts`: \[BigNumberish, BigNumberish\]\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:1049_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1049)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `systemCoin` | string |
| `value` | BigNumberish |
| `minTokenAmounts` | \[BigNumberish, BigNumberish\] |

**Returns:** _TransactionRequest_

### repayAllDebt

▸ **repayAllDebt**\(`safe`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:531_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L531)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |

**Returns:** _TransactionRequest_

### repayAllDebtAndFreeETH

▸ **repayAllDebtAndFreeETH**\(`safe`: BigNumberish, `collateralWad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:541_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L541)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `collateralWad` | BigNumberish |

**Returns:** _TransactionRequest_

### repayAllDebtAndFreeTokenCollateral

▸ **repayAllDebtAndFreeTokenCollateral**\(`collateralJoin`: string, `safe`: BigNumberish, `collateralAmount`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:556_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L556)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `collateralJoin` | string |
| `safe` | BigNumberish |
| `collateralAmount` | BigNumberish |

**Returns:** _TransactionRequest_

### repayDebt

▸ **repayDebt**\(`safe`: BigNumberish, `wad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:572_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L572)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

### repayDebtAndFreeETH

▸ **repayDebtAndFreeETH**\(`safe`: BigNumberish, `collateralWad`: BigNumberish, `deltaWad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:583_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L583)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `collateralWad` | BigNumberish |
| `deltaWad` | BigNumberish |

**Returns:** _TransactionRequest_

### repayDebtAndFreeTokenCollateral

▸ **repayDebtAndFreeTokenCollateral**\(`collateralJoin`: string, `safe`: BigNumberish, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:600_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L600)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `collateralJoin` | string |
| `safe` | BigNumberish |
| `collateralAmount` | BigNumberish |
| `deltaWad` | BigNumberish |

**Returns:** _TransactionRequest_

### safeLockETH

▸ **safeLockETH**\(`ethValue`: BigNumberish, `safe`: BigNumberish, `owner`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:618_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L618)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `ethValue` | BigNumberish |
| `safe` | BigNumberish |
| `owner` | string |

**Returns:** _TransactionRequest_

### safeLockTokenCollateral

▸ **safeLockTokenCollateral**\(`collateralJoin`: string, `safe`: BigNumberish, `amt`: BigNumberish, `transferFrom`: boolean, `owner`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:634_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L634)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `collateralJoin` | string |
| `safe` | BigNumberish |
| `amt` | BigNumberish |
| `transferFrom` | boolean |
| `owner` | string |

**Returns:** _TransactionRequest_

### safeRepayAllDebt

▸ **safeRepayAllDebt**\(`safe`: BigNumberish, `owner`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:653_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L653)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `owner` | string |

**Returns:** _TransactionRequest_

### safeRepayDebt

▸ **safeRepayDebt**\(`safe`: BigNumberish, `wad`: BigNumberish, `owner`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:664_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L664)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `wad` | BigNumberish |
| `owner` | string |

**Returns:** _TransactionRequest_

### stakeInMine

▸ **stakeInMine**\(`wad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:1064_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1064)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

### tokenCollateralJoin\_join

▸ **tokenCollateralJoin\_join**\(`apt`: string, `safe`: string, `amt`: BigNumberish, `transferFrom`: boolean\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:680_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L680)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `apt` | string |
| `safe` | string |
| `amt` | BigNumberish |
| `transferFrom` | boolean |

**Returns:** _TransactionRequest_

### transfer

▸ **transfer**\(`collateral`: string, `dst`: string, `amt`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:696_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L696)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `collateral` | string |
| `dst` | string |
| `amt` | BigNumberish |

**Returns:** _TransactionRequest_

### transferCollateral

▸ **transferCollateral**\(`safe`: BigNumberish, `dst`: string, `wad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:706_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L706)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `dst` | string |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

### transferInternalCoins

▸ **transferInternalCoins**\(`safe`: BigNumberish, `dst`: string, `rad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:721_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L721)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `dst` | string |
| `rad` | BigNumberish |

**Returns:** _TransactionRequest_

### transferSAFEOwnership

▸ **transferSAFEOwnership**\(`safe`: BigNumberish, `usr`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:736_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L736)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `usr` | string |

**Returns:** _TransactionRequest_

### transferSAFEOwnershipToProxy

▸ **transferSAFEOwnershipToProxy**\(`safe`: BigNumberish, `dst`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:746_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L746)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `dst` | string |

**Returns:** _TransactionRequest_

### uniswapV2Call

▸ **uniswapV2Call**\(`_sender`: string, `_amount0`: BigNumberish, `_amount1`: BigNumberish, `_data`: BytesLike\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:1256_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1256)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `_sender` | string |
| `_amount0` | BigNumberish |
| `_amount1` | BigNumberish |
| `_data` | BytesLike |

**Returns:** _TransactionRequest_

### withdrawAndHarvest

▸ **withdrawAndHarvest**\(`value`: BigNumberish, `campaignId`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:1073_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1073)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `value` | BigNumberish |
| `campaignId` | BigNumberish |

**Returns:** _TransactionRequest_

### withdrawAndRemoveLiquidity

▸ **withdrawAndRemoveLiquidity**\(`value`: BigNumberish, `minTokenAmounts`: \[BigNumberish, BigNumberish\]\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:1086_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1086)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `value` | BigNumberish |
| `minTokenAmounts` | \[BigNumberish, BigNumberish\] |

**Returns:** _TransactionRequest_

### withdrawFromMine

▸ **withdrawFromMine**\(`value`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:1101_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1101)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `value` | BigNumberish |

**Returns:** _TransactionRequest_

### withdrawRemoveLiquidityRepayDebt

▸ **withdrawRemoveLiquidityRepayDebt**\(`safe`: BigNumberish, `value`: BigNumberish, `minTokenAmounts`: \[BigNumberish, BigNumberish\]\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:1110_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1110)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `value` | BigNumberish |
| `minTokenAmounts` | \[BigNumberish, BigNumberish\] |

**Returns:** _TransactionRequest_

### withdrawRemoveLiquidityRepayDebtFreeETH

▸ **withdrawRemoveLiquidityRepayDebtFreeETH**\(`safe`: BigNumberish, `valueToWithdraw`: BigNumberish, `ethToFree`: BigNumberish, `minTokenAmounts`: \[BigNumberish, BigNumberish\]\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:1128_](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1128)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `valueToWithdraw` | BigNumberish |
| `ethToFree` | BigNumberish |
| `minTokenAmounts` | \[BigNumberish, BigNumberish\] |

**Returns:** _TransactionRequest_

