# Class: GebProxyActionsGlobalSettlement

Convenience class used to call functions from [GebProxyActionsGlobalSettlement](https://github.com/reflexer-labs/geb-proxy-actions/blob/master/src/GebProxyActions.sol) using a proxy registered in the [GebProxyRegistry](https://github.com/reflexer-labs/geb-proxy-registry/blob/master/src/GebProxyRegistry.sol).
Useful only during Global Settlement in order for users to redeem collateral.

## Examples

Redeem some ETH collateral against some RAI using a proxy contract:
```typescript
// The wallet needs to have a proxy already deployed
const globalSettlementProxy = await geb.getProxyActionGlobalSettlement(wallet.address)
// We need the address of the collateral adapter
const wethJoinAddress = geb.contracts.joinETH_A.address
// Prepare the transaction to redeem 10 RAI for Ether
const tx = globalSettlementProxy.redeemTokenCollateral(wethJoinAddress, ETH_A, WAD.mul(10))
// Send the transaction with a Ethers Wallet object
await wallet.sendTransaction(tx)
```

Redeem as much collateral as possible from a Safe managed by a proxy:
```typescript
// The Safe has to be managed by a proxy for this to work
const globalSettlementProxy = await geb.getProxyActionGlobalSettlement(wallet.address)
// We need the address of the collateral adapter
const wethJoinAddress = geb.contracts.joinETH_A.address
// Extract the collateral from the Safe with ID 3
const tx = globalSettlementProxy.freeTokenCollateral(wethJoinAddress, 3)
// Send the transaction with a Ethers Wallet object
wallet.sendTransaction(tx)
```

## Constructors


\+ **new GebProxyActionsGlobalSettlement**(`proxyAddress`: string, `network`: GebDeployment, `chainProvider`: GebProviderInterface): *[GebProxyActionsGlobalSettlement](gebproxyactionsglobalsettlement.md)*


*Defined in [packages/geb/src/proxy-action-global-settlement.ts:59](https://github.com/reflexer-labs/geb.js/blob/198bcb4/packages/geb/src/proxy-action-global-settlement.ts#L59)*

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

*Defined in [packages/geb/src/proxy-action-global-settlement.ts:53](https://github.com/reflexer-labs/geb.js/blob/198bcb4/packages/geb/src/proxy-action-global-settlement.ts#L53)*

Underlying proxy object. Can be used to make custom calls to the proxy using `proxy.execute()` .
For the details of each function

___

###  proxyActionAddress

• **proxyActionAddress**: *string*

*Defined in [packages/geb/src/proxy-action-global-settlement.ts:58](https://github.com/reflexer-labs/geb.js/blob/198bcb4/packages/geb/src/proxy-action-global-settlement.ts#L58)*

Address of the proxy actions global settlement contract.

___

###  proxyAddress

• **proxyAddress**: *string*

*Defined in [packages/geb/src/proxy-action-global-settlement.ts:65](https://github.com/reflexer-labs/geb.js/blob/198bcb4/packages/geb/src/proxy-action-global-settlement.ts#L65)*

Address of the underlying proxy.

## Methods

###  coinJoin_join

▸ **coinJoin_join**(`apt`: string, `safeHandler`: string, `wad`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action-global-settlement.ts:91](https://github.com/reflexer-labs/geb.js/blob/198bcb4/packages/geb/src/proxy-action-global-settlement.ts#L91)*

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


*Defined in [packages/geb/src/proxy-action-global-settlement.ts:102](https://github.com/reflexer-labs/geb.js/blob/198bcb4/packages/geb/src/proxy-action-global-settlement.ts#L102)*

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


*Defined in [packages/geb/src/proxy-action-global-settlement.ts:118](https://github.com/reflexer-labs/geb.js/blob/198bcb4/packages/geb/src/proxy-action-global-settlement.ts#L118)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |
`safe` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  prepareCoinsForRedeeming

▸ **prepareCoinsForRedeeming**(`wad`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action-global-settlement.ts:133](https://github.com/reflexer-labs/geb.js/blob/198bcb4/packages/geb/src/proxy-action-global-settlement.ts#L133)*

**Parameters:**

Name | Type |
------ | ------ |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  redeemETH

▸ **redeemETH**(`ethJoin`: string, `collateralType`: BytesLike, `wad`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action-global-settlement.ts:144](https://github.com/reflexer-labs/geb.js/blob/198bcb4/packages/geb/src/proxy-action-global-settlement.ts#L144)*

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


*Defined in [packages/geb/src/proxy-action-global-settlement.ts:160](https://github.com/reflexer-labs/geb.js/blob/198bcb4/packages/geb/src/proxy-action-global-settlement.ts#L160)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |
`collateralType` | BytesLike |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*
