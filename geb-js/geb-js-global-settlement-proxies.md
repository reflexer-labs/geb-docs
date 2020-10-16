# Class: GebProxyActionsGlobalSettlement

Convenience class used to call functions from [GebProxyActionsGlobalSettlement](https://github.com/reflexer-labs/geb-proxy-actions/blob/master/src/GebProxyActions.sol) using a proxy registered in the [GebProxyRegistry](https://github.com/reflexer-labs/geb-proxy-registry/blob/master/src/GebProxyRegistry.sol).
Useful only during Global Settlement in order for users to redeem collateral.

## Global settlement guide

Protocol token holders and/or governance can trigger the Global settlement (GS). The procedure is explain in details [on the module page](https://docs.reflexer.finance/system-contracts/shutdown-module/global-settlement#the-shutdown-mechanism-9-crucial-steps). The global starts when the `shutdownSystem()` function of the [global settlement contract](https://github.com/reflexer-labs/geb/blob/38665149f953e14ab19a41f577e42f8f0b565226/src/GlobalSettlement.sol#L254) was called.
To check if the procedure was started do
```typescript
const gsStarted = geb.contracts.globalSettlement.shutdownTime().gt(0)
```

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


*Defined in [packages/geb/src/proxy-action-global-settlement.ts:67](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action-global-settlement.ts#L67)*

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

*Defined in [packages/geb/src/proxy-action-global-settlement.ts:61](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action-global-settlement.ts#L61)*

Underlying proxy object. Can be used to make custom calls to the proxy using `proxy.execute()` .
For the details of each function

___

###  proxyActionAddress

• **proxyActionAddress**: *string*

*Defined in [packages/geb/src/proxy-action-global-settlement.ts:66](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action-global-settlement.ts#L66)*

Address of the proxy actions global settlement contract.

___

###  proxyAddress

• **proxyAddress**: *string*

*Defined in [packages/geb/src/proxy-action-global-settlement.ts:73](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action-global-settlement.ts#L73)*

Address of the underlying proxy.

## Methods

###  coinJoin_join

▸ **coinJoin_join**(`apt`: string, `safeHandler`: string, `wad`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action-global-settlement.ts:99](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action-global-settlement.ts#L99)*

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


*Defined in [packages/geb/src/proxy-action-global-settlement.ts:110](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action-global-settlement.ts#L110)*

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


*Defined in [packages/geb/src/proxy-action-global-settlement.ts:126](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action-global-settlement.ts#L126)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |
`safe` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  prepareCoinsForRedeeming

▸ **prepareCoinsForRedeeming**(`wad`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action-global-settlement.ts:141](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action-global-settlement.ts#L141)*

**Parameters:**

Name | Type |
------ | ------ |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*

___

###  redeemETH

▸ **redeemETH**(`ethJoin`: string, `collateralType`: BytesLike, `wad`: BigNumberish): *TransactionRequest*


*Defined in [packages/geb/src/proxy-action-global-settlement.ts:152](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action-global-settlement.ts#L152)*

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


*Defined in [packages/geb/src/proxy-action-global-settlement.ts:168](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb/src/proxy-action-global-settlement.ts#L168)*

**Parameters:**

Name | Type |
------ | ------ |
`collateralJoin` | string |
`collateralType` | BytesLike |
`wad` | BigNumberish |

**Returns:** *TransactionRequest*
