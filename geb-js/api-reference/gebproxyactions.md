# Class: GebProxyActions

Convenience class to call functions from [GebProxyActions](https://github.com/reflexer-labs/geb-proxy-actions/blob/master/src/GebProxyActions.sol) through a proxy contract registered in the [GebProxyRegistry](https://github.com/reflexer-labs/geb-proxy-registry/blob/master/src/GebProxyRegistry.sol). These actions bundle multiple actions in one (e.g: open a safe + lock some ETH + draw some RAI).

## Constructors


\+ **new GebProxyActions**(`proxyAddress`: string, `network`: GebDeployment, `chainProvider`: GebProviderInterface): *[GebProxyActions](gebproxyactions.md)*

*Defined in [packages/geb/src/proxy-action.ts:57](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L57)*

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

*Defined in [packages/geb/src/proxy-action.ts:28](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L28)*

Underlying proxy object. Can be use to make custom calls to the proxy using `proxy.execute()` function.
For the details of each function

___

###  proxyActionCoreAddress

• **proxyActionCoreAddress**: *string*

*Defined in [packages/geb/src/proxy-action.ts:33](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L33)*

Address of the base proxy action contract.

___

###  proxyActionGlobalSettlementAddress

• **proxyActionGlobalSettlementAddress**: *string*

*Defined in [packages/geb/src/proxy-action.ts:38](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L38)*

Address of the proxy action contract for global settlement.

___

###  proxyActionIncentiveAddress

• **proxyActionIncentiveAddress**: *string*

*Defined in [packages/geb/src/proxy-action.ts:43](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L43)*

Address of the proxy action contract for uniswap LP share staking.

___

###  proxyActionLeverageAddress

• **proxyActionLeverageAddress**: *string*

*Defined in [packages/geb/src/proxy-action.ts:48](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L48)*

Address of the proxy action contract for leveraged with flash loans operations.

___

###  proxyAddress

• **proxyAddress**: *string*

*Defined in [packages/geb/src/proxy-action.ts:62](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L62)*

Address of the underlying proxy

## Methods

###  allowSAFE

▸ **allowSAFE**(`safe`: BigNumberish, `usr`: string, `ok`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:115](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L115)*

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

*Defined in [packages/geb/src/proxy-action.ts:130](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L130)*

**Parameters:**

Name | Type |
------ | ------ |
`obj` | string |
`usr` | string |

**Returns:** *TransactionRequest*

___

###  coinJoin_join

▸ **coinJoin_join**(`apt`: string, `safeHandler`: string, `wad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:136](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L136)*

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

*Defined in [packages/geb/src/proxy-action.ts:146](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L146)*

**Parameters:**

Name | Type |
------ | ------ |
`obj` | string |
`usr` | string |

**Returns:** *TransactionRequest*

___

###  enterSystem

▸ **enterSystem**(`src`: string, `safe`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:152](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L152)*

**Parameters:**

Name | Type |
------ | ------ |
`src` | string |
`safe` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  exitETH

▸ **exitETH**(`safe`: BigNumberish, `wad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:162](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L162)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  exitTokenCollateral

▸ **exitTokenCollateral**(`collateralJoin`: string, `safe`: BigNumberish, `amt`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:173](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L173)*

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

*Defined in [packages/geb/src/proxy-action.ts:815](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L815)*

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

*Defined in [packages/geb/src/proxy-action.ts:836](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L836)*

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

*Defined in [packages/geb/src/proxy-action.ts:859](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L859)*

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

*Defined in [packages/geb/src/proxy-action.ts:188](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L188)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  freeTokenCollateral

▸ **freeTokenCollateral**(`collateralJoin`: string, `safe`: BigNumberish, `amt`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:199](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L199)*

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

*Defined in [packages/geb/src/proxy-action.ts:765](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L765)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |
`safe` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  generateDebt

▸ **generateDebt**(`safe`: BigNumberish, `wad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:214](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L214)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  generateDebtAndProtectSAFE

▸ **generateDebtAndProtectSAFE**(`safe`: BigNumberish, `wad`: BigNumberish, `saviour`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:226](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L226)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`wad` | BigNumberish |
`saviour` | string |

**Returns:** *TransactionRequest*

___

###  lockETH

▸ **lockETH**(`ethValue`: BigNumberish, `safe`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:244](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L244)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`safe` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  lockETHAndGenerateDebt

▸ **lockETHAndGenerateDebt**(`ethValue`: BigNumberish, `safe`: BigNumberish, `deltaWad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:255](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L255)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`safe` | BigNumberish |
`deltaWad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  lockETHLeverage

▸ **lockETHLeverage**(`ethValue`: BigNumberish, `uniswapV2Pair`: string, `callbackProxy`: string, `collateralType`: BytesLike, `safe`: BigNumberish, `leverage`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:882](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L882)*

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

*Defined in [packages/geb/src/proxy-action.ts:273](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L273)*

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

*Defined in [packages/geb/src/proxy-action.ts:290](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L290)*

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

*Defined in [packages/geb/src/proxy-action.ts:311](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L311)*

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

*Defined in [packages/geb/src/proxy-action.ts:335](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L335)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |

**Returns:** *TransactionRequest*

___

###  modifySAFECollateralization

▸ **modifySAFECollateralization**(`safe`: BigNumberish, `deltaCollateral`: BigNumberish, `deltaDebt`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:341](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L341)*

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

*Defined in [packages/geb/src/proxy-action.ts:356](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L356)*

**Parameters:**

Name | Type |
------ | ------ |
`safeSrc` | BigNumberish |
`safeDst` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  openLockETHAndGenerateDebt

▸ **openLockETHAndGenerateDebt**(`ethValue`: BigNumberish, `collateralType`: BytesLike, `deltaWad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:366](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L366)*

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

*Defined in [packages/geb/src/proxy-action.ts:384](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L384)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`collateralType` | BytesLike |
`deltaWad` | BigNumberish |
`saviour` | string |

**Returns:** *TransactionRequest*

___

###  openLockETHLeverage

▸ **openLockETHLeverage**(`ethValue`: BigNumberish, `uniswapV2Pair`: string, `callbackProxy`: string, `collateralType`: BytesLike, `leverage`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:907](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L907)*

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

*Defined in [packages/geb/src/proxy-action.ts:405](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L405)*

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

*Defined in [packages/geb/src/proxy-action.ts:424](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L424)*

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

*Defined in [packages/geb/src/proxy-action.ts:446](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L446)*

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

*Defined in [packages/geb/src/proxy-action.ts:467](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L467)*

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

*Defined in [packages/geb/src/proxy-action.ts:491](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L491)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralType` | BytesLike |
`usr` | string |

**Returns:** *TransactionRequest*

___

###  prepareCoinsForRedeemingGlobalSettlement

▸ **prepareCoinsForRedeemingGlobalSettlement**(`wad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:753](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L753)*

**Parameters:**

Name | Type |
------ | ------ |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  protectSAFE

▸ **protectSAFE**(`safe`: BigNumberish, `saviour`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:501](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L501)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`saviour` | string |

**Returns:** *TransactionRequest*

___

###  quitSystem

▸ **quitSystem**(`safe`: BigNumberish, `dst`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:512](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L512)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`dst` | string |

**Returns:** *TransactionRequest*

___

###  redeemETHGlobalSettlement

▸ **redeemETHGlobalSettlement**(`ethJoin`: string, `collateralType`: BytesLike, `wad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:779](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L779)*

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

*Defined in [packages/geb/src/proxy-action.ts:794](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L794)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |
`collateralType` | BytesLike |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  repayAllDebt

▸ **repayAllDebt**(`safe`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:522](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L522)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  repayAllDebtAndFreeETH

▸ **repayAllDebtAndFreeETH**(`safe`: BigNumberish, `collateralWad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:532](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L532)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`collateralWad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  repayAllDebtAndFreeTokenCollateral

▸ **repayAllDebtAndFreeTokenCollateral**(`collateralJoin`: string, `safe`: BigNumberish, `collateralAmount`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:547](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L547)*

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

*Defined in [packages/geb/src/proxy-action.ts:563](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L563)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  repayDebtAndFreeETH

▸ **repayDebtAndFreeETH**(`safe`: BigNumberish, `collateralWad`: BigNumberish, `deltaWad`: BigNumberish): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:574](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L574)*

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

*Defined in [packages/geb/src/proxy-action.ts:591](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L591)*

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

*Defined in [packages/geb/src/proxy-action.ts:609](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L609)*

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

*Defined in [packages/geb/src/proxy-action.ts:625](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L625)*

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

*Defined in [packages/geb/src/proxy-action.ts:644](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L644)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`owner` | string |

**Returns:** *TransactionRequest*

___

###  safeRepayDebt

▸ **safeRepayDebt**(`safe`: BigNumberish, `wad`: BigNumberish, `owner`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:655](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L655)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`wad` | BigNumberish |
`owner` | string |

**Returns:** *TransactionRequest*

___

###  tokenCollateralJoin_join

▸ **tokenCollateralJoin_join**(`apt`: string, `safe`: string, `amt`: BigNumberish, `transferFrom`: boolean): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:671](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L671)*

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

*Defined in [packages/geb/src/proxy-action.ts:687](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L687)*

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

*Defined in [packages/geb/src/proxy-action.ts:697](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L697)*

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

*Defined in [packages/geb/src/proxy-action.ts:712](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L712)*

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

*Defined in [packages/geb/src/proxy-action.ts:727](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L727)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`usr` | string |

**Returns:** *TransactionRequest*

___

###  transferSAFEOwnershipToProxy

▸ **transferSAFEOwnershipToProxy**(`safe`: BigNumberish, `dst`: string): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:737](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L737)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`dst` | string |

**Returns:** *TransactionRequest*

___

###  uniswapV2Call

▸ **uniswapV2Call**(`_sender`: string, `_amount0`: BigNumberish, `_amount1`: BigNumberish, `_data`: BytesLike): *TransactionRequest*

*Defined in [packages/geb/src/proxy-action.ts:930](https://github.com/reflexer-labs/geb.js/blob/30c41df/packages/geb/src/proxy-action.ts#L930)*

**Parameters:**

Name | Type |
------ | ------ |
`_sender` | string |
`_amount0` | BigNumberish |
`_amount1` | BigNumberish |
`_data` | BytesLike |

**Returns:** *TransactionRequest*
