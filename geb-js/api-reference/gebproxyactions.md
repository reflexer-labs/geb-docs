# Class: GebProxyActions

Convenience class to call functions from [GebProxyActions](https://github.com/reflexer-labs/geb-proxy-actions/blob/master/src/GebProxyActions.sol) through a proxy contract registered in the [GebProxyRegistry](https://github.com/reflexer-labs/geb-proxy-registry/blob/master/src/GebProxyRegistry.sol). These actions bundle multiple actions in one (e.g: open a safe + lock some ETH + draw some RAI).

## Constructors


\+ **new GebProxyActions**(`proxyAddress`: string, `network`: GebDeployment, `chainProvider`: GebProviderInterface): *[GebProxyActions](gebproxyactions.md)*

*Defined in [packages/geb/src/proxy-action.ts:55](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L55)*

**Parameters:**

Name | Type |
------ | ------ |
`proxyAddress` | string |
`network` | GebDeployment |
`chainProvider` | GebProviderInterface |

**Returns:** *[GebProxyActions](gebproxyactions.md)*

## Properties

###  proxy

• **proxy**: *DsProxy*

*Defined in [packages/geb/src/proxy-action.ts:29](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L29)*

Underlying proxy object. Can be use to make custom calls to the proxy using `proxy.execute()` function.
For the details of each function

___

###  proxyActionCoreAddress

• **proxyActionCoreAddress**: *string*

*Defined in [packages/geb/src/proxy-action.ts:34](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L34)*

Address of the base proxy action contract.

___

###  proxyActionGlobalSettlementAddress

• **proxyActionGlobalSettlementAddress**: *string*

*Defined in [packages/geb/src/proxy-action.ts:39](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L39)*

Address of the proxy action contract for global settlement.

___

###  proxyActionIncentiveAddress

• **proxyActionIncentiveAddress**: *string*

*Defined in [packages/geb/src/proxy-action.ts:44](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L44)*

Address of the proxy action contract for uniswap LP share staking.

___

###  proxyActionLeverageAddress

• **proxyActionLeverageAddress**: *string*

*Defined in [packages/geb/src/proxy-action.ts:49](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L49)*

Address of the proxy action contract for leveraged with flash loans operations.

___

###  proxyAddress

• **proxyAddress**: *string*

*Defined in [packages/geb/src/proxy-action.ts:60](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L60)*

Address of the underlying proxy

## Methods

###  allowHandler

▸ **allowHandler**(`usr`: string, `ok`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:820](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L820)*

**Parameters:**

Name | Type |
------ | ------ |
`usr` | string |
`ok` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  allowSAFE

▸ **allowSAFE**(`safe`: BigNumberish, `usr`: string, `ok`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:110](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L110)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`usr` | string |
`ok` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  approveSAFEModification

▸ **approveSAFEModification**(`obj`: string, `usr`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:125](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L125)*

**Parameters:**

Name | Type |
------ | ------ |
`obj` | string |
`usr` | string |

**Returns:** *TransactionRequest*

___

###  coinJoin_join

▸ **coinJoin_join**(`apt`: string, `safeHandler`: string, `wad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:131](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L131)*

**Parameters:**

Name | Type |
------ | ------ |
`apt` | string |
`safeHandler` | string |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  denySAFEModification

▸ **denySAFEModification**(`obj`: string, `usr`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:141](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L141)*

**Parameters:**

Name | Type |
------ | ------ |
`obj` | string |
`usr` | string |

**Returns:** *TransactionRequest*

___

###  enterSystem

▸ **enterSystem**(`src`: string, `safe`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:147](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L147)*

**Parameters:**

Name | Type |
------ | ------ |
`src` | string |
`safe` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  ethJoin_join

▸ **ethJoin_join**(`ethValue`: BigNumberish, `apt`: string, `safe`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:157](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L157)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`apt` | string |
`safe` | string |

**Returns:** *TransactionRequest*

___

###  exitAndRemoveLiquidity

▸ **exitAndRemoveLiquidity**(`minTokenAmounts`: [BigNumberish, BigNumberish]): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:830](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L830)*

**Parameters:**

Name | Type |
------ | ------ |
`minTokenAmounts` | [BigNumberish, BigNumberish] |

**Returns:** *TransactionRequest*

___

###  exitETH

▸ **exitETH**(`safe`: BigNumberish, `wad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:171](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L171)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  exitMine

▸ **exitMine**(`incentives`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:843](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L843)*

**Parameters:**

Name | Type |
------ | ------ |
`incentives` | string |

**Returns:** *TransactionRequest*

___

###  exitRemoveLiquidityRepayDebt

▸ **exitRemoveLiquidityRepayDebt**(`safe`: BigNumberish, `minTokenAmounts`: [BigNumberish, BigNumberish]): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:849](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L849)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`minTokenAmounts` | [BigNumberish, BigNumberish] |

**Returns:** *TransactionRequest*

___

###  exitRemoveLiquidityRepayDebtFreeETH

▸ **exitRemoveLiquidityRepayDebtFreeETH**(`safe`: BigNumberish, `ethToFree`: BigNumberish, `minTokenAmounts`: [BigNumberish, BigNumberish]): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:865](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L865)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`ethToFree` | BigNumberish |
`minTokenAmounts` | [BigNumberish, BigNumberish] |

**Returns:** *TransactionRequest*

___

###  exitTokenCollateral

▸ **exitTokenCollateral**(`collateralJoin`: string, `safe`: BigNumberish, `amt`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:182](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L182)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |
`safe` | BigNumberish |
`amt` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  flashDeleverage

▸ **flashDeleverage**(`uniswapV2Pair`: string, `callbackProxy`: string, `safe`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1151](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1151)*

**Parameters:**

Name | Type |
------ | ------ |
`uniswapV2Pair` | string |
`callbackProxy` | string |
`safe` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  flashDeleverageFreeETH

▸ **flashDeleverageFreeETH**(`uniswapV2Pair`: string, `callbackProxy`: string, `safe`: BigNumberish, `amountToFree`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1170](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1170)*

**Parameters:**

Name | Type |
------ | ------ |
`uniswapV2Pair` | string |
`callbackProxy` | string |
`safe` | BigNumberish |
`amountToFree` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  flashLeverage

▸ **flashLeverage**(`uniswapV2Pair`: string, `callbackProxy`: string, `safe`: BigNumberish, `leverage`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1191](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1191)*

**Parameters:**

Name | Type |
------ | ------ |
`uniswapV2Pair` | string |
`callbackProxy` | string |
`safe` | BigNumberish |
`leverage` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  freeETH

▸ **freeETH**(`safe`: BigNumberish, `wad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:197](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L197)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  freeTokenCollateral

▸ **freeTokenCollateral**(`collateralJoin`: string, `safe`: BigNumberish, `amt`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:208](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L208)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |
`safe` | BigNumberish |
`amt` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  freeTokenCollateralGlobalSettlement

▸ **freeTokenCollateralGlobalSettlement**(`collateralJoin`: string, `safe`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:774](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L774)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |
`safe` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  generateDebt

▸ **generateDebt**(`safe`: BigNumberish, `wad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:223](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L223)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  generateDebtAndProtectSAFE

▸ **generateDebtAndProtectSAFE**(`safe`: BigNumberish, `wad`: BigNumberish, `saviour`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:235](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L235)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`wad` | BigNumberish |
`saviour` | string |

**Returns:** *TransactionRequest*

___

###  generateDebtAndProvideLiquidityStake

▸ **generateDebtAndProvideLiquidityStake**(`ethValue`: BigNumberish, `safe`: BigNumberish, `wad`: BigNumberish, `minTokenAmounts`: [BigNumberish, BigNumberish]): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:884](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L884)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`safe` | BigNumberish |
`wad` | BigNumberish |
`minTokenAmounts` | [BigNumberish, BigNumberish] |

**Returns:** *TransactionRequest*

___

###  generateDebtAndProvideLiquidityUniswap

▸ **generateDebtAndProvideLiquidityUniswap**(`ethValue`: BigNumberish, `safe`: BigNumberish, `wad`: BigNumberish, `minTokenAmounts`: [BigNumberish, BigNumberish]): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:905](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L905)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`safe` | BigNumberish |
`wad` | BigNumberish |
`minTokenAmounts` | [BigNumberish, BigNumberish] |

**Returns:** *TransactionRequest*

___

###  getLockedReward

▸ **getLockedReward**(`campaignId`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:925](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L925)*

**Parameters:**

Name | Type |
------ | ------ |
`campaignId` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  harvestReward

▸ **harvestReward**(`campaignId`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:934](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L934)*

**Parameters:**

Name | Type |
------ | ------ |
`campaignId` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  lockETH

▸ **lockETH**(`ethValue`: BigNumberish, `safe`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:253](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L253)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`safe` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  lockETHAndGenerateDebt

▸ **lockETHAndGenerateDebt**(`ethValue`: BigNumberish, `safe`: BigNumberish, `deltaWad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:264](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L264)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`safe` | BigNumberish |
`deltaWad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  lockETHGenerateDebtProvideLiquidityStake

▸ **lockETHGenerateDebtProvideLiquidityStake**(`ethValue`: BigNumberish, `safe`: BigNumberish, `deltaWad`: BigNumberish, `liquidityWad`: BigNumberish, `minTokenAmounts`: [BigNumberish, BigNumberish]): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:943](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L943)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`safe` | BigNumberish |
`deltaWad` | BigNumberish |
`liquidityWad` | BigNumberish |
`minTokenAmounts` | [BigNumberish, BigNumberish] |

**Returns:** *TransactionRequest*

___

###  lockETHGenerateDebtProvideLiquidityUniswap

▸ **lockETHGenerateDebtProvideLiquidityUniswap**(`ethValue`: BigNumberish, `safe`: BigNumberish, `deltaWad`: BigNumberish, `liquidityWad`: BigNumberish, `minTokenAmounts`: [BigNumberish, BigNumberish]): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:967](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L967)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`safe` | BigNumberish |
`deltaWad` | BigNumberish |
`liquidityWad` | BigNumberish |
`minTokenAmounts` | [BigNumberish, BigNumberish] |

**Returns:** *TransactionRequest*

___

###  lockETHLeverage

▸ **lockETHLeverage**(`ethValue`: BigNumberish, `uniswapV2Pair`: string, `callbackProxy`: string, `safe`: BigNumberish, `leverage`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1212](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1212)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`uniswapV2Pair` | string |
`callbackProxy` | string |
`safe` | BigNumberish |
`leverage` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  lockTokenCollateral

▸ **lockTokenCollateral**(`collateralJoin`: string, `safe`: BigNumberish, `amt`: BigNumberish, `transferFrom`: boolean): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:282](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L282)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |
`safe` | BigNumberish |
`amt` | BigNumberish |
`transferFrom` | boolean |

**Returns:** *TransactionRequest*

___

###  lockTokenCollateralAndGenerateDebt

▸ **lockTokenCollateralAndGenerateDebt**(`collateralJoin`: string, `safe`: BigNumberish, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish, `transferFrom`: boolean): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:299](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L299)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |
`safe` | BigNumberish |
`collateralAmount` | BigNumberish |
`deltaWad` | BigNumberish |
`transferFrom` | boolean |

**Returns:** *TransactionRequest*

___

###  lockTokenCollateralGenerateDebtAndProtectSAFE

▸ **lockTokenCollateralGenerateDebtAndProtectSAFE**(`collateralJoin`: string, `safe`: BigNumberish, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish, `transferFrom`: boolean, `saviour`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:320](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L320)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |
`safe` | BigNumberish |
`collateralAmount` | BigNumberish |
`deltaWad` | BigNumberish |
`transferFrom` | boolean |
`saviour` | string |

**Returns:** *TransactionRequest*

___

###  makeCollateralBag

▸ **makeCollateralBag**(`collateralJoin`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:344](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L344)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |

**Returns:** *TransactionRequest*

___

###  modifySAFECollateralization

▸ **modifySAFECollateralization**(`safe`: BigNumberish, `deltaCollateral`: BigNumberish, `deltaDebt`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:350](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L350)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`deltaCollateral` | BigNumberish |
`deltaDebt` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  moveSAFE

▸ **moveSAFE**(`safeSrc`: BigNumberish, `safeDst`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:365](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L365)*

**Parameters:**

Name | Type |
------ | ------ |
`safeSrc` | BigNumberish |
`safeDst` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  openLockETHAndGenerateDebt

▸ **openLockETHAndGenerateDebt**(`ethValue`: BigNumberish, `collateralType`: BytesLike, `deltaWad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:375](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L375)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`collateralType` | BytesLike |
`deltaWad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  openLockETHGenerateDebtAndProtectSAFE

▸ **openLockETHGenerateDebtAndProtectSAFE**(`ethValue`: BigNumberish, `collateralType`: BytesLike, `deltaWad`: BigNumberish, `saviour`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:393](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L393)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`collateralType` | BytesLike |
`deltaWad` | BigNumberish |
`saviour` | string |

**Returns:** *TransactionRequest*

___

###  openLockETHGenerateDebtProvideLiquidityStake

▸ **openLockETHGenerateDebtProvideLiquidityStake**(`ethValue`: BigNumberish, `deltaWad`: BigNumberish, `liquidityWad`: BigNumberish, `minTokenAmounts`: [BigNumberish, BigNumberish]): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:990](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L990)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`deltaWad` | BigNumberish |
`liquidityWad` | BigNumberish |
`minTokenAmounts` | [BigNumberish, BigNumberish] |

**Returns:** *TransactionRequest*

___

###  openLockETHGenerateDebtProvideLiquidityUniswap

▸ **openLockETHGenerateDebtProvideLiquidityUniswap**(`ethValue`: BigNumberish, `deltaWad`: BigNumberish, `liquidityWad`: BigNumberish, `minTokenAmounts`: [BigNumberish, BigNumberish]): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1012](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1012)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`deltaWad` | BigNumberish |
`liquidityWad` | BigNumberish |
`minTokenAmounts` | [BigNumberish, BigNumberish] |

**Returns:** *TransactionRequest*

___

###  openLockETHLeverage

▸ **openLockETHLeverage**(`ethValue`: BigNumberish, `uniswapV2Pair`: string, `callbackProxy`: string, `leverage`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1235](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1235)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`uniswapV2Pair` | string |
`callbackProxy` | string |
`leverage` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  openLockGNTAndGenerateDebt

▸ **openLockGNTAndGenerateDebt**(`gntJoin`: string, `collateralType`: BytesLike, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:414](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L414)*

**Parameters:**

Name | Type |
------ | ------ |
`gntJoin` | string |
`collateralType` | BytesLike |
`collateralAmount` | BigNumberish |
`deltaWad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  openLockGNTGenerateDebtAndProtectSAFE

▸ **openLockGNTGenerateDebtAndProtectSAFE**(`gntJoin`: string, `collateralType`: BytesLike, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish, `saviour`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:433](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L433)*

**Parameters:**

Name | Type |
------ | ------ |
`gntJoin` | string |
`collateralType` | BytesLike |
`collateralAmount` | BigNumberish |
`deltaWad` | BigNumberish |
`saviour` | string |

**Returns:** *TransactionRequest*

___

###  openLockTokenCollateralAndGenerateDebt

▸ **openLockTokenCollateralAndGenerateDebt**(`collateralJoin`: string, `collateralType`: BytesLike, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish, `transferFrom`: boolean): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:455](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L455)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |
`collateralType` | BytesLike |
`collateralAmount` | BigNumberish |
`deltaWad` | BigNumberish |
`transferFrom` | boolean |

**Returns:** *TransactionRequest*

___

###  openLockTokenCollateralGenerateDebtAndProtectSAFE

▸ **openLockTokenCollateralGenerateDebtAndProtectSAFE**(`collateralJoin`: string, `collateralType`: BytesLike, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish, `transferFrom`: boolean, `saviour`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:476](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L476)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |
`collateralType` | BytesLike |
`collateralAmount` | BigNumberish |
`deltaWad` | BigNumberish |
`transferFrom` | boolean |
`saviour` | string |

**Returns:** *TransactionRequest*

___

###  openSAFE

▸ **openSAFE**(`collateralType`: BytesLike, `usr`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:500](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L500)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralType` | BytesLike |
`usr` | string |

**Returns:** *TransactionRequest*

___

###  prepareCoinsForRedeemingGlobalSettlement

▸ **prepareCoinsForRedeemingGlobalSettlement**(`wad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:762](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L762)*

**Parameters:**

Name | Type |
------ | ------ |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  protectSAFE

▸ **protectSAFE**(`safe`: BigNumberish, `saviour`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:510](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L510)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`saviour` | string |

**Returns:** *TransactionRequest*

___

###  provideLiquidityUniswap

▸ **provideLiquidityUniswap**(`ethValue`: BigNumberish, `wad`: BigNumberish, `minTokenAmounts`: [BigNumberish, BigNumberish]): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1033](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1033)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`wad` | BigNumberish |
`minTokenAmounts` | [BigNumberish, BigNumberish] |

**Returns:** *TransactionRequest*

___

###  quitSystem

▸ **quitSystem**(`safe`: BigNumberish, `dst`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:521](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L521)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`dst` | string |

**Returns:** *TransactionRequest*

___

###  redeemETHGlobalSettlement

▸ **redeemETHGlobalSettlement**(`ethJoin`: string, `collateralType`: BytesLike, `wad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:788](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L788)*

**Parameters:**

Name | Type |
------ | ------ |
`ethJoin` | string |
`collateralType` | BytesLike |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  redeemTokenCollateralGlobalSettlement

▸ **redeemTokenCollateralGlobalSettlement**(`collateralJoin`: string, `collateralType`: BytesLike, `wad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:803](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L803)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |
`collateralType` | BytesLike |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  removeLiquidityUniswap

▸ **removeLiquidityUniswap**(`systemCoin`: string, `value`: BigNumberish, `minTokenAmounts`: [BigNumberish, BigNumberish]): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1049](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1049)*

**Parameters:**

Name | Type |
------ | ------ |
`systemCoin` | string |
`value` | BigNumberish |
`minTokenAmounts` | [BigNumberish, BigNumberish] |

**Returns:** *TransactionRequest*

___

###  repayAllDebt

▸ **repayAllDebt**(`safe`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:531](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L531)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  repayAllDebtAndFreeETH

▸ **repayAllDebtAndFreeETH**(`safe`: BigNumberish, `collateralWad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:541](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L541)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`collateralWad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  repayAllDebtAndFreeTokenCollateral

▸ **repayAllDebtAndFreeTokenCollateral**(`collateralJoin`: string, `safe`: BigNumberish, `collateralAmount`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:556](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L556)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |
`safe` | BigNumberish |
`collateralAmount` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  repayDebt

▸ **repayDebt**(`safe`: BigNumberish, `wad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:572](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L572)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  repayDebtAndFreeETH

▸ **repayDebtAndFreeETH**(`safe`: BigNumberish, `collateralWad`: BigNumberish, `deltaWad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:583](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L583)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`collateralWad` | BigNumberish |
`deltaWad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  repayDebtAndFreeTokenCollateral

▸ **repayDebtAndFreeTokenCollateral**(`collateralJoin`: string, `safe`: BigNumberish, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:600](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L600)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |
`safe` | BigNumberish |
`collateralAmount` | BigNumberish |
`deltaWad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  safeLockETH

▸ **safeLockETH**(`ethValue`: BigNumberish, `safe`: BigNumberish, `owner`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:618](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L618)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`safe` | BigNumberish |
`owner` | string |

**Returns:** *TransactionRequest*

___

###  safeLockTokenCollateral

▸ **safeLockTokenCollateral**(`collateralJoin`: string, `safe`: BigNumberish, `amt`: BigNumberish, `transferFrom`: boolean, `owner`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:634](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L634)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |
`safe` | BigNumberish |
`amt` | BigNumberish |
`transferFrom` | boolean |
`owner` | string |

**Returns:** *TransactionRequest*

___

###  safeRepayAllDebt

▸ **safeRepayAllDebt**(`safe`: BigNumberish, `owner`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:653](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L653)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`owner` | string |

**Returns:** *TransactionRequest*

___

###  safeRepayDebt

▸ **safeRepayDebt**(`safe`: BigNumberish, `wad`: BigNumberish, `owner`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:664](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L664)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`wad` | BigNumberish |
`owner` | string |

**Returns:** *TransactionRequest*

___

###  stakeInMine

▸ **stakeInMine**(`wad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1064](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1064)*

**Parameters:**

Name | Type |
------ | ------ |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  tokenCollateralJoin_join

▸ **tokenCollateralJoin_join**(`apt`: string, `safe`: string, `amt`: BigNumberish, `transferFrom`: boolean): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:680](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L680)*

**Parameters:**

Name | Type |
------ | ------ |
`apt` | string |
`safe` | string |
`amt` | BigNumberish |
`transferFrom` | boolean |

**Returns:** *TransactionRequest*

___

###  transfer

▸ **transfer**(`collateral`: string, `dst`: string, `amt`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:696](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L696)*

**Parameters:**

Name | Type |
------ | ------ |
`collateral` | string |
`dst` | string |
`amt` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  transferCollateral

▸ **transferCollateral**(`safe`: BigNumberish, `dst`: string, `wad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:706](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L706)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`dst` | string |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  transferInternalCoins

▸ **transferInternalCoins**(`safe`: BigNumberish, `dst`: string, `rad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:721](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L721)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`dst` | string |
`rad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  transferSAFEOwnership

▸ **transferSAFEOwnership**(`safe`: BigNumberish, `usr`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:736](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L736)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`usr` | string |

**Returns:** *TransactionRequest*

___

###  transferSAFEOwnershipToProxy

▸ **transferSAFEOwnershipToProxy**(`safe`: BigNumberish, `dst`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:746](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L746)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`dst` | string |

**Returns:** *TransactionRequest*

___

###  uniswapV2Call

▸ **uniswapV2Call**(`_sender`: string, `_amount0`: BigNumberish, `_amount1`: BigNumberish, `_data`: BytesLike): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1256](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1256)*

**Parameters:**

Name | Type |
------ | ------ |
`_sender` | string |
`_amount0` | BigNumberish |
`_amount1` | BigNumberish |
`_data` | BytesLike |

**Returns:** *TransactionRequest*

___

###  withdrawAndHarvest

▸ **withdrawAndHarvest**(`value`: BigNumberish, `campaignId`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1073](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1073)*

**Parameters:**

Name | Type |
------ | ------ |
`value` | BigNumberish |
`campaignId` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  withdrawAndRemoveLiquidity

▸ **withdrawAndRemoveLiquidity**(`value`: BigNumberish, `minTokenAmounts`: [BigNumberish, BigNumberish]): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1086](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1086)*

**Parameters:**

Name | Type |
------ | ------ |
`value` | BigNumberish |
`minTokenAmounts` | [BigNumberish, BigNumberish] |

**Returns:** *TransactionRequest*

___

###  withdrawFromMine

▸ **withdrawFromMine**(`value`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1101](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1101)*

**Parameters:**

Name | Type |
------ | ------ |
`value` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  withdrawRemoveLiquidityRepayDebt

▸ **withdrawRemoveLiquidityRepayDebt**(`safe`: BigNumberish, `value`: BigNumberish, `minTokenAmounts`: [BigNumberish, BigNumberish]): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1110](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1110)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`value` | BigNumberish |
`minTokenAmounts` | [BigNumberish, BigNumberish] |

**Returns:** *TransactionRequest*

___

###  withdrawRemoveLiquidityRepayDebtFreeETH

▸ **withdrawRemoveLiquidityRepayDebtFreeETH**(`safe`: BigNumberish, `valueToWithdraw`: BigNumberish, `ethToFree`: BigNumberish, `minTokenAmounts`: [BigNumberish, BigNumberish]): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1128](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/proxy-action.ts#L1128)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`valueToWithdraw` | BigNumberish |
`ethToFree` | BigNumberish |
`minTokenAmounts` | [BigNumberish, BigNumberish] |

**Returns:** *TransactionRequest*
