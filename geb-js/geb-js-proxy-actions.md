# Class: GebProxyActions

Convenience class to call functions from [GebProxyActions](https://github.com/reflexer-labs/geb-proxy-actions/blob/master/src/GebProxyActions.sol) through a proxy contract registered in the [GebProxyRegistry](https://github.com/reflexer-labs/geb-proxy-registry/blob/master/src/GebProxyRegistry.sol). These actions bundle multiple actions in one (e.g: open a safe + lock some ETH + draw some RAI).

## Constructors


\+ **new GebProxyActions**(`proxyAddress`: string, `network`: GebDeployment, `chainProvider`: GebProviderInterface): *[GebProxyActions](gebproxyactions.md)*


*Defined in [packages/geb/src/proxy-action.ts:33](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L33)*

**Parameters:**

Name | Type |
------ | ------ |
`proxyAddress` | string |
`network` | GebDeployment |
`chainProvider` | GebProviderInterface |

**Returns:** *[GebProxyActions](gebproxyactions.md)*

## Properties

###  address

• **address**: *string*

*Inherited from [GebProxyActions](gebproxyactions.md).[address](gebproxyactions.md#address)*

Defined in packages/geb-contract-base/lib/base-contract-api.d.ts:19

___

###  chainProvider

• **chainProvider**: *GebProviderInterface*

*Inherited from [GebProxyActions](gebproxyactions.md).[chainProvider](gebproxyactions.md#chainprovider)*

Defined in packages/geb-contract-base/lib/base-contract-api.d.ts:20

___

###  proxy

• **proxy**: *DsProxy*

*Defined in [packages/geb/src/proxy-action.ts:27](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L27)*

Underlying proxy object. Can be use to make custom calls to the proxy using `proxy.execute()` function.
For the details of each function

___

###  proxyActionAddress

• **proxyActionAddress**: *string*

*Defined in [packages/geb/src/proxy-action.ts:32](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L32)*

Address of the proxy action contract.

___

###  proxyAddress

• **proxyAddress**: *string*

*Defined in [packages/geb/src/proxy-action.ts:39](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L39)*

Address of the underlying proxy

## Methods

###  allowHandler

▸ **allowHandler**(`usr`: string, `ok`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action.ts:65](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L65)*

**Parameters:**

Name | Type |
------ | ------ |
`usr` | string |
`ok` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  allowSAFE

▸ **allowSAFE**(`safe`: BigNumberish, `usr`: string, `ok`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action.ts:71](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L71)*

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


*Defined in [packages/geb/src/proxy-action.ts:81](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L81)*

**Parameters:**

Name | Type |
------ | ------ |
`obj` | string |
`usr` | string |

**Returns:** *TransactionRequest*

___

###  coinJoin_join

▸ **coinJoin_join**(`apt`: string, `safeHandler`: string, `wad`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action.ts:87](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L87)*

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


*Defined in [packages/geb/src/proxy-action.ts:97](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L97)*

**Parameters:**

Name | Type |
------ | ------ |
`obj` | string |
`usr` | string |

**Returns:** *TransactionRequest*

___

###  enterSystem

▸ **enterSystem**(`src`: string, `safe`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action.ts:103](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L103)*

**Parameters:**

Name | Type |
------ | ------ |
`src` | string |
`safe` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  ethJoin_join

▸ **ethJoin_join**(`ethValue`: BigNumberish, `apt`: string, `safe`: string): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action.ts:109](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L109)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`apt` | string |
`safe` | string |

**Returns:** *TransactionRequest*

___

###  exitETH

▸ **exitETH**(`safe`: BigNumberish, `wad`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action.ts:119](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L119)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  exitTokenCollateral

▸ **exitTokenCollateral**(`collateralJoin`: string, `safe`: BigNumberish, `amt`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action.ts:125](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L125)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |
`safe` | BigNumberish |
`amt` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  freeETH

▸ **freeETH**(`safe`: BigNumberish, `wad`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action.ts:135](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L135)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  freeTokenCollateral

▸ **freeTokenCollateral**(`collateralJoin`: string, `safe`: BigNumberish, `amt`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action.ts:141](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L141)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |
`safe` | BigNumberish |
`amt` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  generateDebt

▸ **generateDebt**(`safe`: BigNumberish, `wad`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action.ts:151](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L151)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  generateDebtAndProtectSAFE

▸ **generateDebtAndProtectSAFE**(`safe`: BigNumberish, `wad`: BigNumberish, `saviour`: string): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action.ts:157](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L157)*

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


*Defined in [packages/geb/src/proxy-action.ts:167](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L167)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`safe` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  lockETHAndGenerateDebt

▸ **lockETHAndGenerateDebt**(`ethValue`: BigNumberish, `safe`: BigNumberish, `deltaWad`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action.ts:173](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L173)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`safe` | BigNumberish |
`deltaWad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  lockTokenCollateral

▸ **lockTokenCollateral**(`manager`: string, `collateralJoin`: string, `safe`: BigNumberish, `amt`: BigNumberish, `transferFrom`: boolean): *TransactionRequest*

*Inherited from [GebProxyActions](gebproxyactions.md).[lockTokenCollateral](gebproxyactions.md#locktokencollateral)*

Defined in packages/geb-contract-api/lib/generated/GebProxyActions.d.ts:21

**Parameters:**

Name | Type |
------ | ------ |
`manager` | string |
`collateralJoin` | string |
`safe` | BigNumberish |
`amt` | BigNumberish |
`transferFrom` | boolean |

**Returns:** *TransactionRequest*

___

###  lockTokenCollateralAndGenerateDebt

▸ **lockTokenCollateralAndGenerateDebt**(`manager`: string, `taxCollector`: string, `collateralJoin`: string, `coinJoin`: string, `safe`: BigNumberish, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish, `transferFrom`: boolean): *TransactionRequest*

*Inherited from [GebProxyActions](gebproxyactions.md).[lockTokenCollateralAndGenerateDebt](gebproxyactions.md#locktokencollateralandgeneratedebt)*

Defined in packages/geb-contract-api/lib/generated/GebProxyActions.d.ts:22

**Parameters:**

Name | Type |
------ | ------ |
`manager` | string |
`taxCollector` | string |
`collateralJoin` | string |
`coinJoin` | string |
`safe` | BigNumberish |
`collateralAmount` | BigNumberish |
`deltaWad` | BigNumberish |
`transferFrom` | boolean |

**Returns:** *TransactionRequest*

___

###  lockTokenCollateralGenerateDebtAndProtectSAFE

▸ **lockTokenCollateralGenerateDebtAndProtectSAFE**(`manager`: string, `taxCollector`: string, `collateralJoin`: string, `coinJoin`: string, `safe`: BigNumberish, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish, `transferFrom`: boolean, `liquidationEngine`: string, `saviour`: string): *TransactionRequest*

*Inherited from [GebProxyActions](gebproxyactions.md).[lockTokenCollateralGenerateDebtAndProtectSAFE](gebproxyactions.md#locktokencollateralgeneratedebtandprotectsafe)*

Defined in packages/geb-contract-api/lib/generated/GebProxyActions.d.ts:23

**Parameters:**

Name | Type |
------ | ------ |
`manager` | string |
`taxCollector` | string |
`collateralJoin` | string |
`coinJoin` | string |
`safe` | BigNumberish |
`collateralAmount` | BigNumberish |
`deltaWad` | BigNumberish |
`transferFrom` | boolean |
`liquidationEngine` | string |
`saviour` | string |

**Returns:** *TransactionRequest*

___

###  makeCollateralBag

▸ **makeCollateralBag**(`collateralJoin`: string): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action.ts:219](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L219)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |

**Returns:** *TransactionRequest*

___

###  modifySAFECollateralization

▸ **modifySAFECollateralization**(`safe`: BigNumberish, `deltaCollateral`: BigNumberish, `deltaDebt`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action.ts:225](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L225)*

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


*Defined in [packages/geb/src/proxy-action.ts:235](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L235)*

**Parameters:**

Name | Type |
------ | ------ |
`safeSrc` | BigNumberish |
`safeDst` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  openLockETHAndGenerateDebt

▸ **openLockETHAndGenerateDebt**(`ethValue`: BigNumberish, `collateralType`: BytesLike, `deltaWad`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action.ts:241](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L241)*

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


*Defined in [packages/geb/src/proxy-action.ts:251](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L251)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`collateralType` | BytesLike |
`deltaWad` | BigNumberish |
`saviour` | string |

**Returns:** *TransactionRequest*

___

###  openLockGNTAndGenerateDebt

▸ **openLockGNTAndGenerateDebt**(`gntJoin`: string, `collateralType`: BytesLike, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action.ts:262](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L262)*

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


*Defined in [packages/geb/src/proxy-action.ts:273](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L273)*

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

▸ **openLockTokenCollateralAndGenerateDebt**(`manager`: string, `taxCollector`: string, `collateralJoin`: string, `coinJoin`: string, `collateralType`: BytesLike, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish, `transferFrom`: boolean): *TransactionRequest*

*Inherited from [GebProxyActions](gebproxyactions.md).[openLockTokenCollateralAndGenerateDebt](gebproxyactions.md#openlocktokencollateralandgeneratedebt)*

Defined in packages/geb-contract-api/lib/generated/GebProxyActions.d.ts:31

**Parameters:**

Name | Type |
------ | ------ |
`manager` | string |
`taxCollector` | string |
`collateralJoin` | string |
`coinJoin` | string |
`collateralType` | BytesLike |
`collateralAmount` | BigNumberish |
`deltaWad` | BigNumberish |
`transferFrom` | boolean |

**Returns:** *TransactionRequest*

___

###  openLockTokenCollateralGenerateDebtAndProtectSAFE

▸ **openLockTokenCollateralGenerateDebtAndProtectSAFE**(`manager`: string, `taxCollector`: string, `collateralJoin`: string, `coinJoin`: string, `collateralType`: BytesLike, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish, `transferFrom`: boolean, `liquidationEngine`: string, `saviour`: string): *TransactionRequest*

*Inherited from [GebProxyActions](gebproxyactions.md).[openLockTokenCollateralGenerateDebtAndProtectSAFE](gebproxyactions.md#openlocktokencollateralgeneratedebtandprotectsafe)*

Defined in packages/geb-contract-api/lib/generated/GebProxyActions.d.ts:32

**Parameters:**

Name | Type |
------ | ------ |
`manager` | string |
`taxCollector` | string |
`collateralJoin` | string |
`coinJoin` | string |
`collateralType` | BytesLike |
`collateralAmount` | BigNumberish |
`deltaWad` | BigNumberish |
`transferFrom` | boolean |
`liquidationEngine` | string |
`saviour` | string |

**Returns:** *TransactionRequest*

___

###  openSAFE

▸ **openSAFE**(`collateralType`: BytesLike, `usr`: string): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action.ts:310](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L310)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralType` | BytesLike |
`usr` | string |

**Returns:** *TransactionRequest*

___

###  protectSAFE

▸ **protectSAFE**(`safe`: BigNumberish, `saviour`: string): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action.ts:316](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L316)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`saviour` | string |

**Returns:** *TransactionRequest*

___

###  quitSystem

▸ **quitSystem**(`safe`: BigNumberish, `dst`: string): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action.ts:322](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L322)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`dst` | string |

**Returns:** *TransactionRequest*

___

###  repayAllDebt

▸ **repayAllDebt**(`safe`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action.ts:328](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L328)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  repayAllDebtAndFreeETH

▸ **repayAllDebtAndFreeETH**(`safe`: BigNumberish, `collateralWad`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action.ts:334](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L334)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`collateralWad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  repayAllDebtAndFreeTokenCollateral

▸ **repayAllDebtAndFreeTokenCollateral**(`collateralJoin`: string, `safe`: BigNumberish, `collateralAmount`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action.ts:343](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L343)*

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


*Defined in [packages/geb/src/proxy-action.ts:353](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L353)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  repayDebtAndFreeETH

▸ **repayDebtAndFreeETH**(`safe`: BigNumberish, `collateralWad`: BigNumberish, `deltaWad`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action.ts:359](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L359)*

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


*Defined in [packages/geb/src/proxy-action.ts:369](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L369)*

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


*Defined in [packages/geb/src/proxy-action.ts:380](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L380)*

**Parameters:**

Name | Type |
------ | ------ |
`ethValue` | BigNumberish |
`safe` | BigNumberish |
`owner` | string |

**Returns:** *TransactionRequest*

___

###  safeLockTokenCollateral

▸ **safeLockTokenCollateral**(`manager`: string, `collateralJoin`: string, `safe`: BigNumberish, `amt`: BigNumberish, `transferFrom`: boolean, `owner`: string): *TransactionRequest*

*Inherited from [GebProxyActions](gebproxyactions.md).[safeLockTokenCollateral](gebproxyactions.md#safelocktokencollateral)*

Defined in packages/geb-contract-api/lib/generated/GebProxyActions.d.ts:43

**Parameters:**

Name | Type |
------ | ------ |
`manager` | string |
`collateralJoin` | string |
`safe` | BigNumberish |
`amt` | BigNumberish |
`transferFrom` | boolean |
`owner` | string |

**Returns:** *TransactionRequest*

___

###  safeRepayAllDebt

▸ **safeRepayAllDebt**(`safe`: BigNumberish, `owner`: string): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action.ts:402](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L402)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`owner` | string |

**Returns:** *TransactionRequest*

___

###  safeRepayDebt

▸ **safeRepayDebt**(`safe`: BigNumberish, `wad`: BigNumberish, `owner`: string): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action.ts:408](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L408)*

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


*Defined in [packages/geb/src/proxy-action.ts:418](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L418)*

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


*Defined in [packages/geb/src/proxy-action.ts:429](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L429)*

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


*Defined in [packages/geb/src/proxy-action.ts:439](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L439)*

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


*Defined in [packages/geb/src/proxy-action.ts:449](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L449)*

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


*Defined in [packages/geb/src/proxy-action.ts:459](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L459)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`usr` | string |

**Returns:** *TransactionRequest*

___

###  transferSAFEOwnershipToProxy

▸ **transferSAFEOwnershipToProxy**(`safe`: BigNumberish, `dst`: string): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action.ts:465](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action.ts#L465)*

**Parameters:**

Name | Type |
------ | ------ |
`safe` | BigNumberish |
`dst` | string |

**Returns:** *TransactionRequest*
