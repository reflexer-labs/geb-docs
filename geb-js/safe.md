# Class: Safe

Represent a GEB safe. Has the safe state and provide helper function to calculate liquidation price, CRatio, etc...

## Constructors


\+ **new Safe**(`contracts`: ContractApis, `handler`: string, `debt`: BigNumber, `collateral`: BigNumber, `collateralType`: string, `isManaged`: boolean): *[Safe](safe.md)*

*Defined in [packages/geb/src/schema/safe.ts:8](https://github.com/reflexer-labs/geb.js/blob/30014ce/packages/geb/src/schema/safe.ts#L8)*

**Parameters:**

Name | Type |
------ | ------ |
`contracts` | ContractApis |
`handler` | string |
`debt` | BigNumber |
`collateral` | BigNumber |
`collateralType` | string |
`isManaged` | boolean |

**Returns:** *[Safe](safe.md)*

## Properties

###  collateral

• **collateral**: *BigNumber*

*Defined in [packages/geb/src/schema/safe.ts:23](https://github.com/reflexer-labs/geb.js/blob/30014ce/packages/geb/src/schema/safe.ts#L23)*

Amount of collateral of the safe as a WAD

___

###  collateralType

• **collateralType**: *string*

*Defined in [packages/geb/src/schema/safe.ts:27](https://github.com/reflexer-labs/geb.js/blob/30014ce/packages/geb/src/schema/safe.ts#L27)*

Collateral type of the safe (ETH-A only)

___

###  debt

• **debt**: *BigNumber*

*Defined in [packages/geb/src/schema/safe.ts:19](https://github.com/reflexer-labs/geb.js/blob/30014ce/packages/geb/src/schema/safe.ts#L19)*

Amount of debt of the safe as a WAD

___

###  handler

• **handler**: *string*

*Defined in [packages/geb/src/schema/safe.ts:15](https://github.com/reflexer-labs/geb.js/blob/30014ce/packages/geb/src/schema/safe.ts#L15)*

Safe handler in safe engine

___

###  isManaged

• **isManaged**: *boolean*

*Defined in [packages/geb/src/schema/safe.ts:31](https://github.com/reflexer-labs/geb.js/blob/30014ce/packages/geb/src/schema/safe.ts#L31)*

If the safe was open through the safe manager

## Methods

###  getCRatio

▸ **getCRatio**(): *Promise‹FixedNumber | null›*

*Defined in [packages/geb/src/schema/safe.ts:38](https://github.com/reflexer-labs/geb.js/blob/30014ce/packages/geb/src/schema/safe.ts#L38)*

Ratio used to calculate the amount of debt that can be drawn. Returns null is ratio is +Infinity. !! Use unsafe division leading to precision loss.

**Returns:** *Promise‹FixedNumber | null›*

Promise<FixedNumber> CRatio

___

###  getLRatio

▸ **getLRatio**(): *Promise‹FixedNumber | null›*

*Defined in [packages/geb/src/schema/safe.ts:61](https://github.com/reflexer-labs/geb.js/blob/30014ce/packages/geb/src/schema/safe.ts#L61)*

Ratio used for liquidation. If LRatio = 1 you can get liquidated, the greater LRatio the safer your safe is. !! Use unsafe division leading to precision loss.

**Returns:** *Promise‹FixedNumber | null›*

Promise<FixedNumber> LRatio

___

###  liquidationPrice

▸ **liquidationPrice**(): *Promise‹FixedNumber | null›*

*Defined in [packages/geb/src/schema/safe.ts:84](https://github.com/reflexer-labs/geb.js/blob/30014ce/packages/geb/src/schema/safe.ts#L84)*

Price at which the safe would get liquidated.

**Returns:** *Promise‹FixedNumber | null›*

<FixedNumber> Liquidation price
