# Class: GebProxyActions

Convenience class to call functions from [GebProxyActions](https://github.com/reflexer-labs/geb-proxy-actions/blob/master/src/GebProxyActions.sol) through a proxy contract registered in the [GebProxyRegistry](https://github.com/reflexer-labs/geb-proxy-registry/blob/master/src/GebProxyRegistry.sol). These actions bundle multiple actions in one (e.g: open a safe + lock some ETH + draw some RAI).

## Constructors


\+ **new GebProxyActions**(`proxyAddress`: string, `network`: GebDeployment, `chainProvider`: GebProviderInterface): *[GebProxyActions](gebproxyactions.md)*

*Defined in [packages/geb/src/proxy-action.ts:57](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L57)*

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

*Defined in [packages/geb/src/proxy-action.ts:29](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L29)*

Underlying proxy object. Can be use to make custom calls to the proxy using `proxy.execute()` function.
For the details of each function

___

###  proxyActionCoreAddress

• **proxyActionCoreAddress**: *string*

*Defined in [packages/geb/src/proxy-action.ts:34](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L34)*

Address of the base proxy action contract.

___

###  proxyActionGlobalSettlementAddress

• **proxyActionGlobalSettlementAddress**: *string*

*Defined in [packages/geb/src/proxy-action.ts:39](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L39)*

Address of the proxy action contract for global settlement.

___

###  proxyActionIncentiveAddress

• **proxyActionIncentiveAddress**: *string*

*Defined in [packages/geb/src/proxy-action.ts:44](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L44)*

Address of the proxy action contract for uniswap LP share staking.

___

###  proxyActionLeverageAddress

• **proxyActionLeverageAddress**: *string*

*Defined in [packages/geb/src/proxy-action.ts:49](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L49)*

Address of the proxy action contract for leveraged with flash loans operations.

___

###  proxyAddress

• **proxyAddress**: *string*

*Defined in [packages/geb/src/proxy-action.ts:62](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L62)*

Address of the underlying proxy

## Methods

###  allowSAFE

▸ **allowSAFE**(`safe`: BigNumberish, `usr`: string, `ok`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:114](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L114)*

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

*Defined in [packages/geb/src/proxy-action.ts:129](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L129)*

**Parameters:**

Name | Type |
------ | ------ |
`obj` | string |
`usr` | string |

**Returns:** *TransactionRequest*

___

###  coinJoin_join

▸ **coinJoin_join**(`apt`: string, `safeHandler`: string, `wad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:135](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L135)*

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

*Defined in [packages/geb/src/proxy-action.ts:145](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L145)*

**Parameters:**

Name | Type |
------ | ------ |
`obj` | string |
`usr` | string |

**Returns:** *TransactionRequest*

___

###  enterSystem

▸ **enterSystem**(`src`: string, `safe`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:151](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L151)*

**Parameters:**

Name | Type |
------ | ------ |
`src` | string |
`safe` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  exitAndRemoveLiquidity

▸ **exitAndRemoveLiquidity**(`minTokenAmounts`: [BigNumberish, BigNumberish], `campaignAddress`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:810](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L810)*

**Parameters:**

Name | Type |
------ | ------ |
`minTokenAmounts` | [BigNumberish, BigNumberish] |
`campaignAddress` | string |

**Returns:** *TransactionRequest*

___

###  exitETH

▸ **exitETH**(`safe`: BigNumberish, `wad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:161](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L161)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  exitMine

▸ **exitMine**(`campaignAddress`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:824](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L824)*

**Parameters:**

Name | Type |
------ | ------ |
`campaignAddress` | string |

**Returns:** *TransactionRequest*

___

###  exitRemoveLiquidityRepayDebt

▸ **exitRemoveLiquidityRepayDebt**(`safe`: BigNumberish, `minTokenAmounts`: [BigNumberish, BigNumberish], `campaignAddress`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:830](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L830)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`minTokenAmounts` | [BigNumberish, BigNumberish] |
`campaignAddress` | string |

**Returns:** *TransactionRequest*

___

###  exitTokenCollateral

▸ **exitTokenCollateral**(`collateralJoin`: string, `safe`: BigNumberish, `amt`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:172](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L172)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |
`safe` | BigNumberish |
`amt` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  flashDeleverage

▸ **flashDeleverage**(`uniswapV2Pair`: string, `callbackProxy`: string, `collateralType`: BytesLike, `safe`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1133](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L1133)*

**Parameters:**

Name | Type |
------ | ------ |
`uniswapV2Pair` | string |
`callbackProxy` | string |
`collateralType` | BytesLike |
`safe` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  flashDeleverageFreeETH

▸ **flashDeleverageFreeETH**(`uniswapV2Pair`: string, `callbackProxy`: string, `collateralType`: BytesLike, `safe`: BigNumberish, `amountToFree`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1154](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L1154)*

**Parameters:**

Name | Type |
------ | ------ |
`uniswapV2Pair` | string |
`callbackProxy` | string |
`collateralType` | BytesLike |
`safe` | BigNumberish |
`amountToFree` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  flashLeverage

▸ **flashLeverage**(`uniswapV2Pair`: string, `callbackProxy`: string, `collateralType`: BytesLike, `safe`: BigNumberish, `leverage`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1177](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L1177)*

**Parameters:**

Name | Type |
------ | ------ |
`uniswapV2Pair` | string |
`callbackProxy` | string |
`collateralType` | BytesLike |
`safe` | BigNumberish |
`leverage` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  freeETH

▸ **freeETH**(`safe`: BigNumberish, `wad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:187](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L187)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  freeTokenCollateral

▸ **freeTokenCollateral**(`collateralJoin`: string, `safe`: BigNumberish, `amt`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:198](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L198)*

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

*Defined in [packages/geb/src/proxy-action.ts:764](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L764)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |
`safe` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  generateDebt

▸ **generateDebt**(`safe`: BigNumberish, `wad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:213](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L213)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  generateDebtAndProtectSAFE

▸ **generateDebtAndProtectSAFE**(`safe`: BigNumberish, `wad`: BigNumberish, `saviour`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:225](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L225)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`wad` | BigNumberish |
`saviour` | string |

**Returns:** *TransactionRequest*

___

###  generateDebtAndProvideLiquidityStake

▸ **generateDebtAndProvideLiquidityStake**(`ethValue`: BigNumberish, `safe`: BigNumberish, `wad`: BigNumberish, `minTokenAmounts`: [BigNumberish, BigNumberish], `campaignAddress`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:847](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L847)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`safe` | BigNumberish |
`wad` | BigNumberish |
`minTokenAmounts` | [BigNumberish, BigNumberish] |
`campaignAddress` | string |

**Returns:** *TransactionRequest*

___

###  generateDebtAndProvideLiquidityUniswap

▸ **generateDebtAndProvideLiquidityUniswap**(`ethValue`: BigNumberish, `safe`: BigNumberish, `wad`: BigNumberish, `minTokenAmounts`: [BigNumberish, BigNumberish], `campaignAddress`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:869](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L869)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`safe` | BigNumberish |
`wad` | BigNumberish |
`minTokenAmounts` | [BigNumberish, BigNumberish] |
`campaignAddress` | string |

**Returns:** *TransactionRequest*

___

###  getRewards

▸ **getRewards**(`campaignAddress`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1079](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L1079)*

**Parameters:**

Name | Type |
------ | ------ |
`campaignAddress` | string |

**Returns:** *TransactionRequest*

___

###  lockETH

▸ **lockETH**(`ethValue`: BigNumberish, `safe`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:243](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L243)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`safe` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  lockETHAndGenerateDebt

▸ **lockETHAndGenerateDebt**(`ethValue`: BigNumberish, `safe`: BigNumberish, `deltaWad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:254](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L254)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`safe` | BigNumberish |
`deltaWad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  lockETHGenerateDebtProvideLiquidityStake

▸ **lockETHGenerateDebtProvideLiquidityStake**(`ethValue`: BigNumberish, `safe`: BigNumberish, `deltaWad`: BigNumberish, `liquidityWad`: BigNumberish, `minTokenAmounts`: [BigNumberish, BigNumberish], `campaignAddress`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:890](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L890)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`safe` | BigNumberish |
`deltaWad` | BigNumberish |
`liquidityWad` | BigNumberish |
`minTokenAmounts` | [BigNumberish, BigNumberish] |
`campaignAddress` | string |

**Returns:** *TransactionRequest*

___

###  lockETHGenerateDebtProvideLiquidityUniswap

▸ **lockETHGenerateDebtProvideLiquidityUniswap**(`ethValue`: BigNumberish, `safe`: BigNumberish, `deltaWad`: BigNumberish, `liquidityWad`: BigNumberish, `minTokenAmounts`: [BigNumberish, BigNumberish]): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:915](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L915)*

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

▸ **lockETHLeverage**(`ethValue`: BigNumberish, `uniswapV2Pair`: string, `callbackProxy`: string, `collateralType`: BytesLike, `safe`: BigNumberish, `leverage`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1200](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L1200)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`uniswapV2Pair` | string |
`callbackProxy` | string |
`collateralType` | BytesLike |
`safe` | BigNumberish |
`leverage` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  lockTokenCollateral

▸ **lockTokenCollateral**(`collateralJoin`: string, `safe`: BigNumberish, `amt`: BigNumberish, `transferFrom`: boolean): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:272](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L272)*

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

*Defined in [packages/geb/src/proxy-action.ts:289](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L289)*

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

*Defined in [packages/geb/src/proxy-action.ts:310](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L310)*

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

*Defined in [packages/geb/src/proxy-action.ts:334](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L334)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |

**Returns:** *TransactionRequest*

___

###  migrateCampaign

▸ **migrateCampaign**(`oldCampaignAddress`: string, `newCampaignAddress`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1119](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L1119)*

**Parameters:**

Name | Type |
------ | ------ |
`oldCampaignAddress` | string |
`newCampaignAddress` | string |

**Returns:** *TransactionRequest*

___

###  modifySAFECollateralization

▸ **modifySAFECollateralization**(`safe`: BigNumberish, `deltaCollateral`: BigNumberish, `deltaDebt`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:340](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L340)*

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

*Defined in [packages/geb/src/proxy-action.ts:355](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L355)*

**Parameters:**

Name | Type |
------ | ------ |
`safeSrc` | BigNumberish |
`safeDst` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  openLockETHAndGenerateDebt

▸ **openLockETHAndGenerateDebt**(`ethValue`: BigNumberish, `collateralType`: BytesLike, `deltaWad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:365](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L365)*

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

*Defined in [packages/geb/src/proxy-action.ts:383](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L383)*

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

▸ **openLockETHGenerateDebtProvideLiquidityStake**(`ethValue`: BigNumberish, `deltaWad`: BigNumberish, `collateralType`: string, `liquidityWad`: BigNumberish, `minTokenAmounts`: [BigNumberish, BigNumberish], `campaignAddress`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:938](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L938)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`deltaWad` | BigNumberish |
`collateralType` | string |
`liquidityWad` | BigNumberish |
`minTokenAmounts` | [BigNumberish, BigNumberish] |
`campaignAddress` | string |

**Returns:** *TransactionRequest*

___

###  openLockETHGenerateDebtProvideLiquidityUniswap

▸ **openLockETHGenerateDebtProvideLiquidityUniswap**(`ethValue`: BigNumberish, `deltaWad`: BigNumberish, `collateralType`: string, `liquidityWad`: BigNumberish, `minTokenAmounts`: [BigNumberish, BigNumberish]): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:963](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L963)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`deltaWad` | BigNumberish |
`collateralType` | string |
`liquidityWad` | BigNumberish |
`minTokenAmounts` | [BigNumberish, BigNumberish] |

**Returns:** *TransactionRequest*

___

###  openLockETHLeverage

▸ **openLockETHLeverage**(`ethValue`: BigNumberish, `uniswapV2Pair`: string, `callbackProxy`: string, `collateralType`: BytesLike, `leverage`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1225](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L1225)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`uniswapV2Pair` | string |
`callbackProxy` | string |
`collateralType` | BytesLike |
`leverage` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  openLockGNTAndGenerateDebt

▸ **openLockGNTAndGenerateDebt**(`gntJoin`: string, `collateralType`: BytesLike, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:404](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L404)*

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

*Defined in [packages/geb/src/proxy-action.ts:423](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L423)*

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

*Defined in [packages/geb/src/proxy-action.ts:445](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L445)*

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

*Defined in [packages/geb/src/proxy-action.ts:466](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L466)*

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

*Defined in [packages/geb/src/proxy-action.ts:490](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L490)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralType` | BytesLike |
`usr` | string |

**Returns:** *TransactionRequest*

___

###  prepareCoinsForRedeemingGlobalSettlement

▸ **prepareCoinsForRedeemingGlobalSettlement**(`wad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:752](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L752)*

**Parameters:**

Name | Type |
------ | ------ |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  protectSAFE

▸ **protectSAFE**(`safe`: BigNumberish, `saviour`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:500](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L500)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`saviour` | string |

**Returns:** *TransactionRequest*

___

###  provideLiquidityStake

▸ **provideLiquidityStake**(`ethValue`: BigNumberish, `wad`: BigNumberish, `minTokenAmounts`: [BigNumberish, BigNumberish], `campaignAddress`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1085](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L1085)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`wad` | BigNumberish |
`minTokenAmounts` | [BigNumberish, BigNumberish] |
`campaignAddress` | string |

**Returns:** *TransactionRequest*

___

###  provideLiquidityUniswap

▸ **provideLiquidityUniswap**(`ethValue`: BigNumberish, `wad`: BigNumberish, `minTokenAmounts`: [BigNumberish, BigNumberish]): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:986](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L986)*

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

*Defined in [packages/geb/src/proxy-action.ts:511](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L511)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`dst` | string |

**Returns:** *TransactionRequest*

___

###  redeemETHGlobalSettlement

▸ **redeemETHGlobalSettlement**(`ethJoin`: string, `collateralType`: BytesLike, `wad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:778](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L778)*

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

*Defined in [packages/geb/src/proxy-action.ts:793](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L793)*

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

*Defined in [packages/geb/src/proxy-action.ts:1002](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L1002)*

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

*Defined in [packages/geb/src/proxy-action.ts:521](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L521)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  repayAllDebtAndFreeETH

▸ **repayAllDebtAndFreeETH**(`safe`: BigNumberish, `collateralWad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:531](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L531)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`collateralWad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  repayAllDebtAndFreeTokenCollateral

▸ **repayAllDebtAndFreeTokenCollateral**(`collateralJoin`: string, `safe`: BigNumberish, `collateralAmount`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:546](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L546)*

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

*Defined in [packages/geb/src/proxy-action.ts:562](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L562)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  repayDebtAndFreeETH

▸ **repayDebtAndFreeETH**(`safe`: BigNumberish, `collateralWad`: BigNumberish, `deltaWad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:573](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L573)*

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

*Defined in [packages/geb/src/proxy-action.ts:590](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L590)*

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

*Defined in [packages/geb/src/proxy-action.ts:608](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L608)*

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

*Defined in [packages/geb/src/proxy-action.ts:624](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L624)*

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

*Defined in [packages/geb/src/proxy-action.ts:643](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L643)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`owner` | string |

**Returns:** *TransactionRequest*

___

###  safeRepayDebt

▸ **safeRepayDebt**(`safe`: BigNumberish, `wad`: BigNumberish, `owner`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:654](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L654)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`wad` | BigNumberish |
`owner` | string |

**Returns:** *TransactionRequest*

___

###  stakeInMine

▸ **stakeInMine**(`wad`: BigNumberish, `campaignAddress`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1017](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L1017)*

**Parameters:**

Name | Type |
------ | ------ |
`wad` | BigNumberish |
`campaignAddress` | string |

**Returns:** *TransactionRequest*

___

###  tokenCollateralJoin_join

▸ **tokenCollateralJoin_join**(`apt`: string, `safe`: string, `amt`: BigNumberish, `transferFrom`: boolean): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:670](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L670)*

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

*Defined in [packages/geb/src/proxy-action.ts:686](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L686)*

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

*Defined in [packages/geb/src/proxy-action.ts:696](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L696)*

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

*Defined in [packages/geb/src/proxy-action.ts:711](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L711)*

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

*Defined in [packages/geb/src/proxy-action.ts:726](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L726)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`usr` | string |

**Returns:** *TransactionRequest*

___

###  transferSAFEOwnershipToProxy

▸ **transferSAFEOwnershipToProxy**(`safe`: BigNumberish, `dst`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:736](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L736)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`dst` | string |

**Returns:** *TransactionRequest*

___

###  uniswapV2Call

▸ **uniswapV2Call**(`_sender`: string, `_amount0`: BigNumberish, `_amount1`: BigNumberish, `_data`: BytesLike): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1248](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L1248)*

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

▸ **withdrawAndHarvest**(`value`: BigNumberish, `campaignAddress`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1026](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L1026)*

**Parameters:**

Name | Type |
------ | ------ |
`value` | BigNumberish |
`campaignAddress` | string |

**Returns:** *TransactionRequest*

___

###  withdrawAndRemoveLiquidity

▸ **withdrawAndRemoveLiquidity**(`value`: BigNumberish, `minTokenAmounts`: [BigNumberish, BigNumberish], `campaignAddress`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1035](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L1035)*

**Parameters:**

Name | Type |
------ | ------ |
`value` | BigNumberish |
`minTokenAmounts` | [BigNumberish, BigNumberish] |
`campaignAddress` | string |

**Returns:** *TransactionRequest*

___

###  withdrawFromMine

▸ **withdrawFromMine**(`value`: BigNumberish, `campaignAddress`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1051](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L1051)*

**Parameters:**

Name | Type |
------ | ------ |
`value` | BigNumberish |
`campaignAddress` | string |

**Returns:** *TransactionRequest*

___

###  withdrawHarvestRemoveLiquidity

▸ **withdrawHarvestRemoveLiquidity**(`amount`: BigNumberish, `minTokenAmounts`: [BigNumberish, BigNumberish], `campaignAddress`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1103](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L1103)*

**Parameters:**

Name | Type |
------ | ------ |
`amount` | BigNumberish |
`minTokenAmounts` | [BigNumberish, BigNumberish] |
`campaignAddress` | string |

**Returns:** *TransactionRequest*

___

###  withdrawRemoveLiquidityRepayDebt

▸ **withdrawRemoveLiquidityRepayDebt**(`safe`: BigNumberish, `value`: BigNumberish, `minTokenAmounts`: [BigNumberish, BigNumberish], `campaignAddress`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:1060](https://github.com/reflexer-labs/geb.js/blob/22d068f/packages/geb/src/proxy-action.ts#L1060)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`value` | BigNumberish |
`minTokenAmounts` | [BigNumberish, BigNumberish] |
`campaignAddress` | string |

**Returns:** *TransactionRequest*
