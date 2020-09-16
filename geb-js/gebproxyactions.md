# SAFE Proxies

Convenience class to call function from Proxy Action contract through a proxy registered in the proxy registry. These action allow to bundle non-view calls. i.g: Open a safe + Lock some ETH + draw some RAI in one single transaction.

## Constructors

+ **new GebProxyActions**\(`proxyAddress`: string, `network`: GebDeployment, `chainProvider`: GebProviderInterface\): [_GebProxyActions_](gebproxyactions.md)

_Defined in_ [_packages/geb/src/proxy-action.ts:33_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L33)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `proxyAddress` | string |
| `network` | GebDeployment |
| `chainProvider` | GebProviderInterface |

**Returns:** [_GebProxyActions_](gebproxyactions.md)

## Properties

### address

• **address**: _string_

_Inherited from_ [_GebProxyActions_](gebproxyactions.md)_._[_address_](gebproxyactions.md#address)

Defined in packages/geb-contract-base/lib/base-contract-api.d.ts:19

### chainProvider

• **chainProvider**: _GebProviderInterface_

_Inherited from_ [_GebProxyActions_](gebproxyactions.md)_._[_chainProvider_](gebproxyactions.md#chainprovider)

Defined in packages/geb-contract-base/lib/base-contract-api.d.ts:20

### proxy

• **proxy**: _DsProxy_

_Defined in_ [_packages/geb/src/proxy-action.ts:27_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L27)

Underlying proxy object. Can be use to make custom calls to the proxy using `proxy.execute()` function. For the details of each function

### proxyActionAddress

• **proxyActionAddress**: _string_

_Defined in_ [_packages/geb/src/proxy-action.ts:32_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L32)

Address of the proxy action contract.

### proxyAddress

• **proxyAddress**: _string_

_Defined in_ [_packages/geb/src/proxy-action.ts:39_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L39)

Address of the underlying proxy

## Methods

### allowHandler

▸ **allowHandler**\(`usr`: string, `ok`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:65_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L65)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `usr` | string |
| `ok` | BigNumberish |

**Returns:** _TransactionRequest_

### allowSAFE

▸ **allowSAFE**\(`safe`: BigNumberish, `usr`: string, `ok`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:71_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L71)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `usr` | string |
| `ok` | BigNumberish |

**Returns:** _TransactionRequest_

### approveSAFEModification

▸ **approveSAFEModification**\(`obj`: string, `usr`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:81_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L81)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `obj` | string |
| `usr` | string |

**Returns:** _TransactionRequest_

### coinJoin\_join

▸ **coinJoin\_join**\(`apt`: string, `safeHandler`: string, `wad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:87_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L87)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `apt` | string |
| `safeHandler` | string |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

### denySAFEModification

▸ **denySAFEModification**\(`obj`: string, `usr`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:97_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L97)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `obj` | string |
| `usr` | string |

**Returns:** _TransactionRequest_

### enterSystem

▸ **enterSystem**\(`src`: string, `safe`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:103_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L103)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `src` | string |
| `safe` | BigNumberish |

**Returns:** _TransactionRequest_

### ethJoin\_join

▸ **ethJoin\_join**\(`ethValue`: BigNumberish, `apt`: string, `safe`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:109_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L109)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `ethValue` | BigNumberish |
| `apt` | string |
| `safe` | string |

**Returns:** _TransactionRequest_

### exitETH

▸ **exitETH**\(`safe`: BigNumberish, `wad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:119_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L119)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

### exitTokenCollateral

▸ **exitTokenCollateral**\(`collateralJoin`: string, `safe`: BigNumberish, `amt`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:125_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L125)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `collateralJoin` | string |
| `safe` | BigNumberish |
| `amt` | BigNumberish |

**Returns:** _TransactionRequest_

### freeETH

▸ **freeETH**\(`safe`: BigNumberish, `wad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:135_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L135)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

### freeTokenCollateral

▸ **freeTokenCollateral**\(`collateralJoin`: string, `safe`: BigNumberish, `amt`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:141_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L141)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `collateralJoin` | string |
| `safe` | BigNumberish |
| `amt` | BigNumberish |

**Returns:** _TransactionRequest_

### generateDebt

▸ **generateDebt**\(`safe`: BigNumberish, `wad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:151_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L151)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

### generateDebtAndProtectSAFE

▸ **generateDebtAndProtectSAFE**\(`safe`: BigNumberish, `wad`: BigNumberish, `saviour`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:157_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L157)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `wad` | BigNumberish |
| `saviour` | string |

**Returns:** _TransactionRequest_

### lockETH

▸ **lockETH**\(`ethValue`: BigNumberish, `safe`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:167_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L167)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `ethValue` | BigNumberish |
| `safe` | BigNumberish |

**Returns:** _TransactionRequest_

### lockETHAndGenerateDebt

▸ **lockETHAndGenerateDebt**\(`ethValue`: BigNumberish, `safe`: BigNumberish, `deltaWad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:173_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L173)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `ethValue` | BigNumberish |
| `safe` | BigNumberish |
| `deltaWad` | BigNumberish |

**Returns:** _TransactionRequest_

### lockTokenCollateral

▸ **lockTokenCollateral**\(`manager`: string, `collateralJoin`: string, `safe`: BigNumberish, `amt`: BigNumberish, `transferFrom`: boolean\): _TransactionRequest_

_Inherited from_ [_GebProxyActions_](gebproxyactions.md)_._[_lockTokenCollateral_](gebproxyactions.md#locktokencollateral)

Defined in packages/geb-contract-api/lib/generated/GebProxyActions.d.ts:21

**Parameters:**

| Name | Type |
| :--- | :--- |
| `manager` | string |
| `collateralJoin` | string |
| `safe` | BigNumberish |
| `amt` | BigNumberish |
| `transferFrom` | boolean |

**Returns:** _TransactionRequest_

### lockTokenCollateralAndGenerateDebt

▸ **lockTokenCollateralAndGenerateDebt**\(`manager`: string, `taxCollector`: string, `collateralJoin`: string, `coinJoin`: string, `safe`: BigNumberish, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish, `transferFrom`: boolean\): _TransactionRequest_

_Inherited from_ [_GebProxyActions_](gebproxyactions.md)_._[_lockTokenCollateralAndGenerateDebt_](gebproxyactions.md#locktokencollateralandgeneratedebt)

Defined in packages/geb-contract-api/lib/generated/GebProxyActions.d.ts:22

**Parameters:**

| Name | Type |
| :--- | :--- |
| `manager` | string |
| `taxCollector` | string |
| `collateralJoin` | string |
| `coinJoin` | string |
| `safe` | BigNumberish |
| `collateralAmount` | BigNumberish |
| `deltaWad` | BigNumberish |
| `transferFrom` | boolean |

**Returns:** _TransactionRequest_

### lockTokenCollateralGenerateDebtAndProtectSAFE

▸ **lockTokenCollateralGenerateDebtAndProtectSAFE**\(`manager`: string, `taxCollector`: string, `collateralJoin`: string, `coinJoin`: string, `safe`: BigNumberish, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish, `transferFrom`: boolean, `liquidationEngine`: string, `saviour`: string\): _TransactionRequest_

_Inherited from_ [_GebProxyActions_](gebproxyactions.md)_._[_lockTokenCollateralGenerateDebtAndProtectSAFE_](gebproxyactions.md#locktokencollateralgeneratedebtandprotectsafe)

Defined in packages/geb-contract-api/lib/generated/GebProxyActions.d.ts:23

**Parameters:**

| Name | Type |
| :--- | :--- |
| `manager` | string |
| `taxCollector` | string |
| `collateralJoin` | string |
| `coinJoin` | string |
| `safe` | BigNumberish |
| `collateralAmount` | BigNumberish |
| `deltaWad` | BigNumberish |
| `transferFrom` | boolean |
| `liquidationEngine` | string |
| `saviour` | string |

**Returns:** _TransactionRequest_

### makeCollateralBag

▸ **makeCollateralBag**\(`collateralJoin`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:219_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L219)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `collateralJoin` | string |

**Returns:** _TransactionRequest_

### modifySAFECollateralization

▸ **modifySAFECollateralization**\(`safe`: BigNumberish, `deltaCollateral`: BigNumberish, `deltaDebt`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:225_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L225)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `deltaCollateral` | BigNumberish |
| `deltaDebt` | BigNumberish |

**Returns:** _TransactionRequest_

### moveSAFE

▸ **moveSAFE**\(`safeSrc`: BigNumberish, `safeDst`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:235_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L235)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safeSrc` | BigNumberish |
| `safeDst` | BigNumberish |

**Returns:** _TransactionRequest_

### openLockETHAndGenerateDebt

▸ **openLockETHAndGenerateDebt**\(`ethValue`: BigNumberish, `collateralType`: BytesLike, `deltaWad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:241_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L241)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `ethValue` | BigNumberish |
| `collateralType` | BytesLike |
| `deltaWad` | BigNumberish |

**Returns:** _TransactionRequest_

### openLockETHGenerateDebtAndProtectSAFE

▸ **openLockETHGenerateDebtAndProtectSAFE**\(`ethValue`: BigNumberish, `collateralType`: BytesLike, `deltaWad`: BigNumberish, `saviour`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:251_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L251)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `ethValue` | BigNumberish |
| `collateralType` | BytesLike |
| `deltaWad` | BigNumberish |
| `saviour` | string |

**Returns:** _TransactionRequest_

### openLockGNTAndGenerateDebt

▸ **openLockGNTAndGenerateDebt**\(`gntJoin`: string, `collateralType`: BytesLike, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:262_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L262)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `gntJoin` | string |
| `collateralType` | BytesLike |
| `collateralAmount` | BigNumberish |
| `deltaWad` | BigNumberish |

**Returns:** _TransactionRequest_

### openLockGNTGenerateDebtAndProtectSAFE

▸ **openLockGNTGenerateDebtAndProtectSAFE**\(`gntJoin`: string, `collateralType`: BytesLike, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish, `saviour`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:273_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L273)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `gntJoin` | string |
| `collateralType` | BytesLike |
| `collateralAmount` | BigNumberish |
| `deltaWad` | BigNumberish |
| `saviour` | string |

**Returns:** _TransactionRequest_

### openLockTokenCollateralAndGenerateDebt

▸ **openLockTokenCollateralAndGenerateDebt**\(`manager`: string, `taxCollector`: string, `collateralJoin`: string, `coinJoin`: string, `collateralType`: BytesLike, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish, `transferFrom`: boolean\): _TransactionRequest_

_Inherited from_ [_GebProxyActions_](gebproxyactions.md)_._[_openLockTokenCollateralAndGenerateDebt_](gebproxyactions.md#openlocktokencollateralandgeneratedebt)

Defined in packages/geb-contract-api/lib/generated/GebProxyActions.d.ts:31

**Parameters:**

| Name | Type |
| :--- | :--- |
| `manager` | string |
| `taxCollector` | string |
| `collateralJoin` | string |
| `coinJoin` | string |
| `collateralType` | BytesLike |
| `collateralAmount` | BigNumberish |
| `deltaWad` | BigNumberish |
| `transferFrom` | boolean |

**Returns:** _TransactionRequest_

### openLockTokenCollateralGenerateDebtAndProtectSAFE

▸ **openLockTokenCollateralGenerateDebtAndProtectSAFE**\(`manager`: string, `taxCollector`: string, `collateralJoin`: string, `coinJoin`: string, `collateralType`: BytesLike, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish, `transferFrom`: boolean, `liquidationEngine`: string, `saviour`: string\): _TransactionRequest_

_Inherited from_ [_GebProxyActions_](gebproxyactions.md)_._[_openLockTokenCollateralGenerateDebtAndProtectSAFE_](gebproxyactions.md#openlocktokencollateralgeneratedebtandprotectsafe)

Defined in packages/geb-contract-api/lib/generated/GebProxyActions.d.ts:32

**Parameters:**

| Name | Type |
| :--- | :--- |
| `manager` | string |
| `taxCollector` | string |
| `collateralJoin` | string |
| `coinJoin` | string |
| `collateralType` | BytesLike |
| `collateralAmount` | BigNumberish |
| `deltaWad` | BigNumberish |
| `transferFrom` | boolean |
| `liquidationEngine` | string |
| `saviour` | string |

**Returns:** _TransactionRequest_

### openSAFE

▸ **openSAFE**\(`collateralType`: BytesLike, `usr`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:310_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L310)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `collateralType` | BytesLike |
| `usr` | string |

**Returns:** _TransactionRequest_

### protectSAFE

▸ **protectSAFE**\(`safe`: BigNumberish, `saviour`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:316_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L316)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `saviour` | string |

**Returns:** _TransactionRequest_

### quitSystem

▸ **quitSystem**\(`safe`: BigNumberish, `dst`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:322_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L322)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `dst` | string |

**Returns:** _TransactionRequest_

### repayAllDebt

▸ **repayAllDebt**\(`safe`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:328_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L328)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |

**Returns:** _TransactionRequest_

### repayAllDebtAndFreeETH

▸ **repayAllDebtAndFreeETH**\(`safe`: BigNumberish, `collateralWad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:334_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L334)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `collateralWad` | BigNumberish |

**Returns:** _TransactionRequest_

### repayAllDebtAndFreeTokenCollateral

▸ **repayAllDebtAndFreeTokenCollateral**\(`collateralJoin`: string, `safe`: BigNumberish, `collateralAmount`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:343_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L343)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `collateralJoin` | string |
| `safe` | BigNumberish |
| `collateralAmount` | BigNumberish |

**Returns:** _TransactionRequest_

### repayDebt

▸ **repayDebt**\(`safe`: BigNumberish, `wad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:353_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L353)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

### repayDebtAndFreeETH

▸ **repayDebtAndFreeETH**\(`safe`: BigNumberish, `collateralWad`: BigNumberish, `deltaWad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:359_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L359)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `collateralWad` | BigNumberish |
| `deltaWad` | BigNumberish |

**Returns:** _TransactionRequest_

### repayDebtAndFreeTokenCollateral

▸ **repayDebtAndFreeTokenCollateral**\(`collateralJoin`: string, `safe`: BigNumberish, `collateralAmount`: BigNumberish, `deltaWad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:369_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L369)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `collateralJoin` | string |
| `safe` | BigNumberish |
| `collateralAmount` | BigNumberish |
| `deltaWad` | BigNumberish |

**Returns:** _TransactionRequest_

### safeLockETH

▸ **safeLockETH**\(`ethValue`: BigNumberish, `safe`: BigNumberish, `owner`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:380_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L380)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `ethValue` | BigNumberish |
| `safe` | BigNumberish |
| `owner` | string |

**Returns:** _TransactionRequest_

### safeLockTokenCollateral

▸ **safeLockTokenCollateral**\(`manager`: string, `collateralJoin`: string, `safe`: BigNumberish, `amt`: BigNumberish, `transferFrom`: boolean, `owner`: string\): _TransactionRequest_

_Inherited from_ [_GebProxyActions_](gebproxyactions.md)_._[_safeLockTokenCollateral_](gebproxyactions.md#safelocktokencollateral)

Defined in packages/geb-contract-api/lib/generated/GebProxyActions.d.ts:43

**Parameters:**

| Name | Type |
| :--- | :--- |
| `manager` | string |
| `collateralJoin` | string |
| `safe` | BigNumberish |
| `amt` | BigNumberish |
| `transferFrom` | boolean |
| `owner` | string |

**Returns:** _TransactionRequest_

### safeRepayAllDebt

▸ **safeRepayAllDebt**\(`safe`: BigNumberish, `owner`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:402_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L402)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `owner` | string |

**Returns:** _TransactionRequest_

### safeRepayDebt

▸ **safeRepayDebt**\(`safe`: BigNumberish, `wad`: BigNumberish, `owner`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:408_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L408)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `wad` | BigNumberish |
| `owner` | string |

**Returns:** _TransactionRequest_

### tokenCollateralJoin\_join

▸ **tokenCollateralJoin\_join**\(`apt`: string, `safe`: string, `amt`: BigNumberish, `transferFrom`: boolean\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:418_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L418)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `apt` | string |
| `safe` | string |
| `amt` | BigNumberish |
| `transferFrom` | boolean |

**Returns:** _TransactionRequest_

### transfer

▸ **transfer**\(`collateral`: string, `dst`: string, `amt`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:429_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L429)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `collateral` | string |
| `dst` | string |
| `amt` | BigNumberish |

**Returns:** _TransactionRequest_

### transferCollateral

▸ **transferCollateral**\(`safe`: BigNumberish, `dst`: string, `wad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:439_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L439)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `dst` | string |
| `wad` | BigNumberish |

**Returns:** _TransactionRequest_

### transferInternalCoins

▸ **transferInternalCoins**\(`safe`: BigNumberish, `dst`: string, `rad`: BigNumberish\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:449_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L449)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `dst` | string |
| `rad` | BigNumberish |

**Returns:** _TransactionRequest_

### transferSAFEOwnership

▸ **transferSAFEOwnership**\(`safe`: BigNumberish, `usr`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:459_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L459)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `usr` | string |

**Returns:** _TransactionRequest_

### transferSAFEOwnershipToProxy

▸ **transferSAFEOwnershipToProxy**\(`safe`: BigNumberish, `dst`: string\): _TransactionRequest_

_Defined in_ [_packages/geb/src/proxy-action.ts:465_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb/src/proxy-action.ts#L465)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `safe` | BigNumberish |
| `dst` | string |

**Returns:** _TransactionRequest_

