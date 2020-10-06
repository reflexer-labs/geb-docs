# Global Settlement Proxies

Convenience class used to call functions from [GebProxyActionsGlobalSettlement](https://github.com/reflexer-labs/geb-proxy-actions/blob/master/src/GebProxyActions.sol) using a proxy registered in the [GebProxyRegistry](https://github.com/reflexer-labs/geb-proxy-registry/blob/master/src/GebProxyRegistry.sol). Useful only during Global Settlement in order for users to redeem collateral.

## Global settlement guide

Protocol token holders and/or governance can trigger the Global settlement \(GS\). The procedure is explain in details [on the module page](https://docs.reflexer.finance/system-contracts/shutdown-module/global-settlement#the-shutdown-mechanism-9-crucial-steps). The global starts when the `shutdownSystem()` function of the [global settlement contract](https://github.com/reflexer-labs/geb/blob/38665149f953e14ab19a41f577e42f8f0b565226/src/GlobalSettlement.sol#L254) was called. To check if the procedure was started do

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

+ **new GebProxyActionsGlobalSettlement**\(`proxyAddress`: string, `network`: GebDeployment, `chainProvider`: GebProviderInterface\): [_GebProxyActionsGlobalSettlement_](https://github.com/reflexer-labs/geb-docs/tree/e7dbbfb802c8df8a5a32e3c071e06b0dd68199c7/geb-js/gebproxyactionsglobalsettlement.md)

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:67_](https://github.com/reflexer-labs/geb.js/blob/8731b9f/packages/geb/src/proxy-action-global-settlement.ts#L67)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `proxyAddress` | string |
| `network` | GebDeployment |
| `chainProvider` | GebProviderInterface |

**Returns:** [_GebProxyActionsGlobalSettlement_](https://github.com/reflexer-labs/geb-docs/tree/e7dbbfb802c8df8a5a32e3c071e06b0dd68199c7/geb-js/gebproxyactionsglobalsettlement.md)

## Properties

### address

• **address**: _string_

_Inherited from_ [_GebProxyActions_](https://github.com/reflexer-labs/geb-docs/tree/e7dbbfb802c8df8a5a32e3c071e06b0dd68199c7/geb-js/gebproxyactions.md)_._[_address_](https://github.com/reflexer-labs/geb-docs/tree/e7dbbfb802c8df8a5a32e3c071e06b0dd68199c7/geb-js/gebproxyactions.md#address)

Defined in packages/geb-contract-base/lib/base-contract-api.d.ts:19

### chainProvider

• **chainProvider**: _GebProviderInterface_

_Inherited from_ [_GebProxyActions_](https://github.com/reflexer-labs/geb-docs/tree/e7dbbfb802c8df8a5a32e3c071e06b0dd68199c7/geb-js/gebproxyactions.md)_._[_chainProvider_](https://github.com/reflexer-labs/geb-docs/tree/e7dbbfb802c8df8a5a32e3c071e06b0dd68199c7/geb-js/gebproxyactions.md#chainprovider)

Defined in packages/geb-contract-base/lib/base-contract-api.d.ts:20

### proxy

• **proxy**: _DsProxy_

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:61_](https://github.com/reflexer-labs/geb.js/blob/8731b9f/packages/geb/src/proxy-action-global-settlement.ts#L61)

Underlying proxy object. Can be used to make custom calls to the proxy using `proxy.execute()` . For the details of each function

### proxyActionAddress

• **proxyActionAddress**: _string_

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:66_](https://github.com/reflexer-labs/geb.js/blob/8731b9f/packages/geb/src/proxy-action-global-settlement.ts#L66)

Address of the proxy actions global settlement contract.

### proxyAddress

• **proxyAddress**: _string_

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:73_](https://github.com/reflexer-labs/geb.js/blob/8731b9f/packages/geb/src/proxy-action-global-settlement.ts#L73)

Address of the underlying proxy.

## Methods

### coinJoin\_join

▸ **coinJoin\_join**\(`apt`: string, `safeHandler`: string, `wad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:99_](https://github.com/reflexer-labs/geb.js/blob/8731b9f/packages/geb/src/proxy-action-global-settlement.ts#L99)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `apt` | string |
| `safeHandler` | string |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

### freeETH

▸ **freeETH**\(`ethJoin`: string, `globalSettlement`: string, `safe`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:110_](https://github.com/reflexer-labs/geb.js/blob/8731b9f/packages/geb/src/proxy-action-global-settlement.ts#L110)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `ethJoin` | string |
| `globalSettlement` | string |
| `safe` | BigNumberish |

**Returns:** _TransactionRequest_

### freeTokenCollateral

▸ **freeTokenCollateral**\(`collateralJoin`: string, `safe`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:126_](https://github.com/reflexer-labs/geb.js/blob/8731b9f/packages/geb/src/proxy-action-global-settlement.ts#L126)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `collateralJoin` | string |
| `safe` | BigNumberish |

**Returns:** _TransactionRequest_

### prepareCoinsForRedeeming

▸ **prepareCoinsForRedeeming**\(`wad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:141_](https://github.com/reflexer-labs/geb.js/blob/8731b9f/packages/geb/src/proxy-action-global-settlement.ts#L141)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

### redeemETH

▸ **redeemETH**\(`ethJoin`: string, `collateralType`: BytesLike, `wad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:152_](https://github.com/reflexer-labs/geb.js/blob/8731b9f/packages/geb/src/proxy-action-global-settlement.ts#L152)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `ethJoin` | string |
| `collateralType` | BytesLike |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

### redeemTokenCollateral

▸ **redeemTokenCollateral**\(`collateralJoin`: string, `collateralType`: BytesLike, `wad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:168_](https://github.com/reflexer-labs/geb.js/blob/8731b9f/packages/geb/src/proxy-action-global-settlement.ts#L168)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `collateralJoin` | string |
| `collateralType` | BytesLike |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

