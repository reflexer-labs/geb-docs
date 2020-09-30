# Global Settlement Proxies

Convenience class used to call functions from [GebProxyActionsGlobalSettlement](https://github.com/reflexer-labs/geb-proxy-actions/blob/master/src/GebProxyActions.sol) using a proxy registered in the [GebProxyRegistry](https://github.com/reflexer-labs/geb-proxy-registry/blob/master/src/GebProxyRegistry.sol). Useful only during Global Settlement in order for users to redeem collateral.

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

+ **new GebProxyActionsGlobalSettlement**\(`proxyAddress`: string, `network`: GebDeployment, `chainProvider`: GebProviderInterface\): [_GebProxyActionsGlobalSettlement_](geb-js-global-settlement-proxies.md)

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:59_](https://github.com/reflexer-labs/geb.js/blob/198bcb4/packages/geb/src/proxy-action-global-settlement.ts#L59)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `proxyAddress` | string |
| `network` | GebDeployment |
| `chainProvider` | GebProviderInterface |

**Returns:** [_GebProxyActionsGlobalSettlement_](geb-js-global-settlement-proxies.md)

## Properties

### address

• **address**: _string_

_Inherited from_ [_GebProxyActions_](geb-js-proxy-actions.md)_._[_address_](geb-js-proxy-actions.md#address)

Defined in packages/geb-contract-base/lib/base-contract-api.d.ts:19

### chainProvider

• **chainProvider**: _GebProviderInterface_

_Inherited from_ [_GebProxyActions_](geb-js-proxy-actions.md)_._[_chainProvider_](geb-js-proxy-actions.md#chainprovider)

Defined in packages/geb-contract-base/lib/base-contract-api.d.ts:20

### proxy

• **proxy**: _DsProxy_

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:53_](https://github.com/reflexer-labs/geb.js/blob/198bcb4/packages/geb/src/proxy-action-global-settlement.ts#L53)

Underlying proxy object. Can be used to make custom calls to the proxy using `proxy.execute()` . For the details of each function

### proxyActionAddress

• **proxyActionAddress**: _string_

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:58_](https://github.com/reflexer-labs/geb.js/blob/198bcb4/packages/geb/src/proxy-action-global-settlement.ts#L58)

Address of the proxy actions global settlement contract.

### proxyAddress

• **proxyAddress**: _string_

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:65_](https://github.com/reflexer-labs/geb.js/blob/198bcb4/packages/geb/src/proxy-action-global-settlement.ts#L65)

Address of the underlying proxy.

## Methods

### coinJoin\_join

▸ **coinJoin\_join**\(`apt`: string, `safeHandler`: string, `wad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:91_](https://github.com/reflexer-labs/geb.js/blob/198bcb4/packages/geb/src/proxy-action-global-settlement.ts#L91)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `apt` | string |
| `safeHandler` | string |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

### freeETH

▸ **freeETH**\(`ethJoin`: string, `globalSettlement`: string, `safe`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:102_](https://github.com/reflexer-labs/geb.js/blob/198bcb4/packages/geb/src/proxy-action-global-settlement.ts#L102)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `ethJoin` | string |
| `globalSettlement` | string |
| `safe` | BigNumberish |

**Returns:** _TransactionRequest_

### freeTokenCollateral

▸ **freeTokenCollateral**\(`collateralJoin`: string, `safe`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:118_](https://github.com/reflexer-labs/geb.js/blob/198bcb4/packages/geb/src/proxy-action-global-settlement.ts#L118)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `collateralJoin` | string |
| `safe` | BigNumberish |

**Returns:** _TransactionRequest_

### prepareCoinsForRedeeming

▸ **prepareCoinsForRedeeming**\(`wad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:133_](https://github.com/reflexer-labs/geb.js/blob/198bcb4/packages/geb/src/proxy-action-global-settlement.ts#L133)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

### redeemETH

▸ **redeemETH**\(`ethJoin`: string, `collateralType`: BytesLike, `wad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:144_](https://github.com/reflexer-labs/geb.js/blob/198bcb4/packages/geb/src/proxy-action-global-settlement.ts#L144)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `ethJoin` | string |
| `collateralType` | BytesLike |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

### redeemTokenCollateral

▸ **redeemTokenCollateral**\(`collateralJoin`: string, `collateralType`: BytesLike, `wad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:160_](https://github.com/reflexer-labs/geb.js/blob/198bcb4/packages/geb/src/proxy-action-global-settlement.ts#L160)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `collateralJoin` | string |
| `collateralType` | BytesLike |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

