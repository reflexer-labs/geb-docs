# Safe

Represent a GEB safe. Has the safe state and provide helper function to calculate liquidation price, CRatio, etc...

## Constructors

+ **new Safe**\(`contracts`: ContractApis, `handler`: string, `debt`: BigNumber, `collateral`: BigNumber, `collateralType`: string, `isManaged`: boolean\): [_Safe_](safe.md)

_Defined in_ [_packages/geb/src/schema/safe.ts:8_](https://github.com/reflexer-labs/geb.js/blob/acffc2e/packages/geb/src/schema/safe.ts#L8)

**Parameters:**

| Name | Type |
| :--- | :--- |
| `contracts` | ContractApis |
| `handler` | string |
| `debt` | BigNumber |
| `collateral` | BigNumber |
| `collateralType` | string |
| `isManaged` | boolean |

**Returns:** [_Safe_](safe.md)

## Properties

### collateral

• **collateral**: _BigNumber_

_Defined in_ [_packages/geb/src/schema/safe.ts:23_](https://github.com/reflexer-labs/geb.js/blob/acffc2e/packages/geb/src/schema/safe.ts#L23)

Amount of collateral of the safe as a WAD

### collateralType

• **collateralType**: _string_

_Defined in_ [_packages/geb/src/schema/safe.ts:27_](https://github.com/reflexer-labs/geb.js/blob/acffc2e/packages/geb/src/schema/safe.ts#L27)

Collateral type of the safe \(ETH-A only\)

### debt

• **debt**: _BigNumber_

_Defined in_ [_packages/geb/src/schema/safe.ts:19_](https://github.com/reflexer-labs/geb.js/blob/acffc2e/packages/geb/src/schema/safe.ts#L19)

Amount of debt of the safe as a WAD

### handler

• **handler**: _string_

_Defined in_ [_packages/geb/src/schema/safe.ts:15_](https://github.com/reflexer-labs/geb.js/blob/acffc2e/packages/geb/src/schema/safe.ts#L15)

Safe handler in safe engine

### isManaged

• **isManaged**: _boolean_

_Defined in_ [_packages/geb/src/schema/safe.ts:31_](https://github.com/reflexer-labs/geb.js/blob/acffc2e/packages/geb/src/schema/safe.ts#L31)

If the safe was open through the safe manager

## Methods

### getCRatio

▸ **getCRatio**\(\): _Promise‹FixedNumber \| null›_

_Defined in_ [_packages/geb/src/schema/safe.ts:38_](https://github.com/reflexer-labs/geb.js/blob/acffc2e/packages/geb/src/schema/safe.ts#L38)

Ratio used to calculate the amount of debt that can be drawn. Returns null is ratio is +Infinity. !! Use unsafe division leading to precision loss.

**Returns:** _Promise‹FixedNumber \| null›_

Promise CRatio

### getLRatio

▸ **getLRatio**\(\): _Promise‹FixedNumber \| null›_

_Defined in_ [_packages/geb/src/schema/safe.ts:61_](https://github.com/reflexer-labs/geb.js/blob/acffc2e/packages/geb/src/schema/safe.ts#L61)

Ratio used for liquidation. If LRatio = 1 you can get liquidated, the greater LRatio the safer your safe is. !! Use unsafe division leading to precision loss.

**Returns:** _Promise‹FixedNumber \| null›_

Promise LRatio

### liquidationPrice

▸ **liquidationPrice**\(\): _Promise‹FixedNumber \| null›_

_Defined in_ [_packages/geb/src/schema/safe.ts:84_](https://github.com/reflexer-labs/geb.js/blob/acffc2e/packages/geb/src/schema/safe.ts#L84)

Price at which the safe would get liquidated.

**Returns:** _Promise‹FixedNumber \| null›_

 Liquidation price

