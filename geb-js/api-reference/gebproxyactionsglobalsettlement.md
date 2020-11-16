# Class: GebProxyActionsGlobalSettlement

Convenience class used to call functions from
[GebProxyActionsGlobalSettlement](https://github.com/reflexer-labs/geb-proxy-actions/blob/master/src/GebProxyActions.sol)
using a proxy registered in the [GebProxyRegistry](https://github.com/reflexer-labs/geb-proxy-registry/blob/master/src/GebProxyRegistry.sol).
Useful only during Global Settlement in order for users to redeem collateral. See the
[Global Settlement Guide](https://docs.reflexer.finance/geb-js/geb-js-global-settlement-guide).

## Constructors


\+ **new GebProxyActionsGlobalSettlement**(`proxyAddress`: string, `network`: GebDeployment, `chainProvider`: GebProviderInterface): *[GebProxyActionsGlobalSettlement](gebproxyactionsglobalsettlement.md)*


*Defined in [packages/geb/src/proxy-action-global-settlement.ts:36](https://github.com/reflexer-labs/geb.js/blob/0337d96/packages/geb/src/proxy-action-global-settlement.ts#L36)*

**Parameters:**

Name | Type |
------ | ------ |
`proxyAddress` | string |
`network` | GebDeployment |
`chainProvider` | GebProviderInterface |

**Returns:** *[GebProxyActionsGlobalSettlement](gebproxyactionsglobalsettlement.md)*

## Properties

###  address

• **address**: *string*

*Inherited from [GebProxyActions](gebproxyactions.md).[address](gebproxyactions.md#address)*

Defined in packages/geb-contract-base/lib/base-contract-api.d.ts:22

___

###  chainProvider

• **chainProvider**: *GebProviderInterface*

*Inherited from [GebProxyActions](gebproxyactions.md).[chainProvider](gebproxyactions.md#chainprovider)*

Defined in packages/geb-contract-base/lib/base-contract-api.d.ts:23

___

###  proxy

• **proxy**: *DsProxy*

*Defined in [packages/geb/src/proxy-action-global-settlement.ts:30](https://github.com/reflexer-labs/geb.js/blob/0337d96/packages/geb/src/proxy-action-global-settlement.ts#L30)*

Underlying proxy object. Can be used to make custom calls to the proxy using `proxy.execute()` .
For the details of each function

___

###  proxyActionAddress

• **proxyActionAddress**: *string*

*Defined in [packages/geb/src/proxy-action-global-settlement.ts:35](https://github.com/reflexer-labs/geb.js/blob/0337d96/packages/geb/src/proxy-action-global-settlement.ts#L35)*

Address of the proxy actions global settlement contract.

___

###  proxyAddress

• **proxyAddress**: *string*

*Defined in [packages/geb/src/proxy-action-global-settlement.ts:42](https://github.com/reflexer-labs/geb.js/blob/0337d96/packages/geb/src/proxy-action-global-settlement.ts#L42)*

Address of the underlying proxy.

## Methods

###  coinJoin_join

▸ **coinJoin_join**(`apt`: string, `safeHandler`: string, `wad`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action-global-settlement.ts:68](https://github.com/reflexer-labs/geb.js/blob/0337d96/packages/geb/src/proxy-action-global-settlement.ts#L68)*

**Parameters:**

Name | Type |
------ | ------ |
`apt` | string |
`safeHandler` | string |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  freeETH

▸ **freeETH**(`ethJoin`: string, `globalSettlement`: string, `safe`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action-global-settlement.ts:79](https://github.com/reflexer-labs/geb.js/blob/0337d96/packages/geb/src/proxy-action-global-settlement.ts#L79)*

**Parameters:**

Name | Type |
------ | ------ |
`ethJoin` | string |
`globalSettlement` | string |
`safe` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  freeTokenCollateral

▸ **freeTokenCollateral**(`collateralJoin`: string, `safe`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action-global-settlement.ts:95](https://github.com/reflexer-labs/geb.js/blob/0337d96/packages/geb/src/proxy-action-global-settlement.ts#L95)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |
`safe` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  prepareCoinsForRedeeming

▸ **prepareCoinsForRedeeming**(`wad`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action-global-settlement.ts:110](https://github.com/reflexer-labs/geb.js/blob/0337d96/packages/geb/src/proxy-action-global-settlement.ts#L110)*

**Parameters:**

Name | Type |
------ | ------ |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  redeemETH

▸ **redeemETH**(`ethJoin`: string, `collateralType`: BytesLike, `wad`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action-global-settlement.ts:121](https://github.com/reflexer-labs/geb.js/blob/0337d96/packages/geb/src/proxy-action-global-settlement.ts#L121)*

**Parameters:**

Name | Type |
------ | ------ |
`ethJoin` | string |
`collateralType` | BytesLike |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  redeemTokenCollateral

▸ **redeemTokenCollateral**(`collateralJoin`: string, `collateralType`: BytesLike, `wad`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action-global-settlement.ts:137](https://github.com/reflexer-labs/geb.js/blob/0337d96/packages/geb/src/proxy-action-global-settlement.ts#L137)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |
`collateralType` | BytesLike |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*
