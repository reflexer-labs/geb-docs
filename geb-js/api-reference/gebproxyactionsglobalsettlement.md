# Proxy Actions Global Settlement

Convenience class used to call functions from [GebProxyActionsGlobalSettlement](https://github.com/reflexer-labs/geb-proxy-actions/blob/master/src/GebProxyActions.sol) using a proxy registered in the [GebProxyRegistry](https://github.com/reflexer-labs/geb-proxy-registry/blob/master/src/GebProxyRegistry.sol). Useful only during Global Settlement in order for users to redeem collateral. See the [Global Settlement Guide](https://docs.reflexer.finance/geb-js/geb-js-global-settlement-guide).

## Constructors

+ **new GebProxyActionsGlobalSettlement**\(`proxyAddress`: string, `network`: GebDeployment, `chainProvider`: GebProviderInterface\): [_GebProxyActionsGlobalSettlement_](gebproxyactionsglobalsettlement.md)

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:36_](https://github.com/reflexer-labs/geb.js/blob/0337d96/packages/geb/src/proxy-action-global-settlement.ts#L36)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `proxyAddress` | string |
| `network` | GebDeployment |
| `chainProvider` | GebProviderInterface |

**Returns:** [_GebProxyActionsGlobalSettlement_](gebproxyactionsglobalsettlement.md)

## Properties

### address

• **address**: _string_

_Inherited from_ [_GebProxyActions_](gebproxyactions.md)_._[_address_](gebproxyactions.md#address)

Defined in packages/geb-contract-base/lib/base-contract-api.d.ts:22

### chainProvider

• **chainProvider**: _GebProviderInterface_

_Inherited from_ [_GebProxyActions_](gebproxyactions.md)_._[_chainProvider_](gebproxyactions.md#chainprovider)

Defined in packages/geb-contract-base/lib/base-contract-api.d.ts:23

### proxy

• **proxy**: _DsProxy_

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:30_](https://github.com/reflexer-labs/geb.js/blob/0337d96/packages/geb/src/proxy-action-global-settlement.ts#L30)

Underlying proxy object. Can be used to make custom calls to the proxy using `proxy.execute()` . For the details of each function

### proxyActionAddress

• **proxyActionAddress**: _string_

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:35_](https://github.com/reflexer-labs/geb.js/blob/0337d96/packages/geb/src/proxy-action-global-settlement.ts#L35)

Address of the proxy actions global settlement contract.

### proxyAddress

• **proxyAddress**: _string_

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:42_](https://github.com/reflexer-labs/geb.js/blob/0337d96/packages/geb/src/proxy-action-global-settlement.ts#L42)

Address of the underlying proxy.

## Methods

### coinJoin\_join

▸ **coinJoin\_join**\(`apt`: string, `safeHandler`: string, `wad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:68_](https://github.com/reflexer-labs/geb.js/blob/0337d96/packages/geb/src/proxy-action-global-settlement.ts#L68)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `apt` | string |
| `safeHandler` | string |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

### freeETH

▸ **freeETH**\(`ethJoin`: string, `globalSettlement`: string, `safe`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:79_](https://github.com/reflexer-labs/geb.js/blob/0337d96/packages/geb/src/proxy-action-global-settlement.ts#L79)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `ethJoin` | string |
| `globalSettlement` | string |
| `safe` | BigNumberish |

**Returns:** _TransactionRequest_

### freeTokenCollateral

▸ **freeTokenCollateral**\(`collateralJoin`: string, `safe`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:95_](https://github.com/reflexer-labs/geb.js/blob/0337d96/packages/geb/src/proxy-action-global-settlement.ts#L95)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `collateralJoin` | string |
| `safe` | BigNumberish |

**Returns:** _TransactionRequest_

### prepareCoinsForRedeeming

▸ **prepareCoinsForRedeeming**\(`wad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:110_](https://github.com/reflexer-labs/geb.js/blob/0337d96/packages/geb/src/proxy-action-global-settlement.ts#L110)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

### redeemETH

▸ **redeemETH**\(`ethJoin`: string, `collateralType`: BytesLike, `wad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:121_](https://github.com/reflexer-labs/geb.js/blob/0337d96/packages/geb/src/proxy-action-global-settlement.ts#L121)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `ethJoin` | string |
| `collateralType` | BytesLike |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

### redeemTokenCollateral

▸ **redeemTokenCollateral**\(`collateralJoin`: string, `collateralType`: BytesLike, `wad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action-global-settlement.ts:137_](https://github.com/reflexer-labs/geb.js/blob/0337d96/packages/geb/src/proxy-action-global-settlement.ts#L137)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `collateralJoin` | string |
| `collateralType` | BytesLike |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

