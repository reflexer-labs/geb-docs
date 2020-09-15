# Class: GebProxyActionsGlobalSettlement

Convenience class to call function from the Proxy Action Global Settlement contract through a proxy registered in the proxy registry. It is used the same way as the `ProxyAction` class.
Useful only during global settlement (System shutdown) for users to retrieve their collateral.

## Constructors


\+ **new GebProxyActionsGlobalSettlement**(`proxyAddress`: string, `network`: GebDeployment, `chainProvider`: GebProviderInterface): *[GebProxyActionsGlobalSettlement](gebproxyactionsglobalsettlement.md)*

*Overrides void*

*Defined in [packages/geb/src/proxy-action-global-settlement.ts:32](https://github.com/reflexer-labs/geb.js/blob/81e18a1/packages/geb/src/proxy-action-global-settlement.ts#L32)*

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

Defined in packages/geb-contract-base/lib/base-contract-api.d.ts:19

___

###  chainProvider

• **chainProvider**: *GebProviderInterface*

*Inherited from [GebProxyActions](gebproxyactions.md).[chainProvider](gebproxyactions.md#chainprovider)*

Defined in packages/geb-contract-base/lib/base-contract-api.d.ts:20

___

###  proxy

• **proxy**: *DsProxy*

*Defined in [packages/geb/src/proxy-action-global-settlement.ts:26](https://github.com/reflexer-labs/geb.js/blob/81e18a1/packages/geb/src/proxy-action-global-settlement.ts#L26)*

Underlying proxy object. Can be use to make custom calls to the proxy using `proxy.execute()` function.
For the details of each function

___

###  proxyActionAddress

• **proxyActionAddress**: *string*

*Defined in [packages/geb/src/proxy-action-global-settlement.ts:31](https://github.com/reflexer-labs/geb.js/blob/81e18a1/packages/geb/src/proxy-action-global-settlement.ts#L31)*

Address of the proxy action global settlement contract.

___

###  proxyAddress

• **proxyAddress**: *string*

*Defined in [packages/geb/src/proxy-action-global-settlement.ts:38](https://github.com/reflexer-labs/geb.js/blob/81e18a1/packages/geb/src/proxy-action-global-settlement.ts#L38)*

Address of the underlying proxy

## Methods

###  coinJoin_join

▸ **coinJoin_join**(`apt`: string, `safeHandler`: string, `wad`: BigNumberish): *TransactionRequest*

*Overrides void*

*Defined in [packages/geb/src/proxy-action-global-settlement.ts:64](https://github.com/reflexer-labs/geb.js/blob/81e18a1/packages/geb/src/proxy-action-global-settlement.ts#L64)*

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

*Overrides void*

*Defined in [packages/geb/src/proxy-action-global-settlement.ts:75](https://github.com/reflexer-labs/geb.js/blob/81e18a1/packages/geb/src/proxy-action-global-settlement.ts#L75)*

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

*Overrides void*

*Defined in [packages/geb/src/proxy-action-global-settlement.ts:91](https://github.com/reflexer-labs/geb.js/blob/81e18a1/packages/geb/src/proxy-action-global-settlement.ts#L91)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |
`safe` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  prepareCoinsForRedeeming

▸ **prepareCoinsForRedeeming**(`wad`: BigNumberish): *TransactionRequest*

*Overrides void*

*Defined in [packages/geb/src/proxy-action-global-settlement.ts:106](https://github.com/reflexer-labs/geb.js/blob/81e18a1/packages/geb/src/proxy-action-global-settlement.ts#L106)*

**Parameters:**

Name | Type |
------ | ------ |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  redeemETH

▸ **redeemETH**(`ethJoin`: string, `collateralType`: BytesLike, `wad`: BigNumberish): *TransactionRequest*

*Overrides void*

*Defined in [packages/geb/src/proxy-action-global-settlement.ts:117](https://github.com/reflexer-labs/geb.js/blob/81e18a1/packages/geb/src/proxy-action-global-settlement.ts#L117)*

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

*Overrides void*

*Defined in [packages/geb/src/proxy-action-global-settlement.ts:133](https://github.com/reflexer-labs/geb.js/blob/81e18a1/packages/geb/src/proxy-action-global-settlement.ts#L133)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |
`collateralType` | BytesLike |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*
