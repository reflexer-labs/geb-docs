# Class: GebAdmin

This class extends the core `GEB` class with additional tools and contracts that are not used as often as other SAFE management tools.
Here you will find utils for contracts such as DSPause, ESM etc. These contracts are scattered across several repositories. Please refer to the smart contract documentation to learn more about them.

**IMPORTANT:** To avoid bloating the main [geb.js](https://www.npmjs.com/package/geb.js) package this class is only available in a [separate package](https://www.npmjs.com/package/@reflexer-finance/geb-admin).
Please install it like this:
```
npm install @reflexer-finance/geb-admin
```

And you are ready to use the admin tools similar to the GEB class:

```typescript
import { ethers } from 'ethers'
import { GebAdmin } from "@reflexer-finance/geb-admin"

 const provider = new ethers.providers.JsonRpcProvider('http://kovan.infura.io/<API KEY>')
 const gebAdmin = new GebAdmin('kovan', provider)
```

## Constructors


\+ **new GebAdmin**(`network`: GebDeployment, `provider`: GebProviderInterface | Provider): *[GebAdmin](gebadmin.md)*


*Defined in [packages/geb-admin/src/geb-admin.ts:51](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb-admin/src/geb-admin.ts#L51)*

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`network` | GebDeployment | Either `'kovan'`, `'mainnet'` or an actual list of contract addresses. |
`provider` | GebProviderInterface &#124; Provider | Either a Ethers.js provider or a GEB provider. Support for Web3.js will soon be added.  |

**Returns:** *[GebAdmin](gebadmin.md)*

## Properties

###  contracts

• **contracts**: *ContractApis*

*Inherited from [GebAdmin](gebadmin.md).[contracts](gebadmin.md#contracts)*

Defined in packages/geb/lib/geb.d.ts:34

Object containing all GEB core contracts instances for low level interactions. All contracts object offer a one-to-one typed API to the underlying smart-contract.
Currently has the following contracts:
- SAFEEngine
- AccountingEngine
- TaxCollector
- LiquidationEngine
- OracleRelayer
- GlobalSettlement
- DebtAuctionHouse
- PreSettlementSurplusAuctionHouse
- PostSettlementSurplusAuctionHouse
- SettlementSurplusAuctioneer
- GebSafeManager
- GetSafes
- BasicCollateralJoin
- CoinJoin
- Coin (RAI ERC20 contract)
- GebProxyRegistry
- FixedDiscountCollateralAuctionHouse (For ETH-A)
- Weth (ERC20)

___

###  contractsAdmin

• **contractsAdmin**: *AdminApis*

*Defined in [packages/geb-admin/src/geb-admin.ts:51](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb-admin/src/geb-admin.ts#L51)*

Object containing all GEB admin contracts instances for low level interactions.
It currently has the following contracts:
- MultiSigWallet
- DsProxy
- DsToken
- ProtocolTokenAuthority
- GebPollingEmitter
- GebPrintingPermissions
- DsDelegateRoles
- DsPause
- DsPauseProxy
- GovActions
- ESM
- TokenBurner
- FsmGovernanceInterface
- DsProxyFactory
- GebDeployPauseProxyActions
- DsProxy
- TxManager

## Methods

###  deployProxy

▸ **deployProxy**(): *object*

*Inherited from [GebAdmin](gebadmin.md).[deployProxy](gebadmin.md#deployproxy)*

Defined in packages/geb/lib/geb.d.ts:57

Deploy a new proxy owned by the sender.

**Returns:** *object*

* **chainId**? : *number*

* **data**? : *string*

* **from**? : *string*

* **gasLimit**? : *BigNumber*

* **gasPrice**? : *BigNumber*

* **nonce**? : *number*

* **to**? : *string*

* **value**? : *BigNumber*

___

###  getErc20Contract

▸ **getErc20Contract**(`tokenAddress`: string): *Erc20*

*Inherited from [GebAdmin](gebadmin.md).[getErc20Contract](gebadmin.md#geterc20contract)*

Defined in packages/geb/lib/geb.d.ts:81

Returns an object that can be used to interact with a ERC20 token.
Example:
```typescript
const USDCAddress = "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48"
const USDC = geb.getErc20Contract(USDCAddress)

// Get 0xdefiisawesome's balance
const balance = USDC.balanceOf("0xdefiisawesome..")

// Send 1 USDC to 0xdefiisawesome (USDC is 6 decimals)
const tx = USDC.transfer("0xdefiisawesome..", "1000000")
await wallet.sendTransaction(tx)
```

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`tokenAddress` | string | Token contract address |

**Returns:** *Erc20*

Erc20

___

###  getProxyAction

▸ **getProxyAction**(`ownerAddress`: string): *Promise‹GebProxyActions›*

*Inherited from [GebAdmin](gebadmin.md).[getProxyAction](gebadmin.md#getproxyaction)*

Defined in packages/geb/lib/geb.d.ts:47

Given an address returns a GebProxyActions object to execute bundled operations.
Important: This requires the address to have deployed a GEB proxy through the proxy registry contract. It will throw a `DOES_NOT_OWN_HAVE_PROXY` error if the address specified does not have a proxy. Use the [deployProxy](gebadmin.md#deployproxy) function to get a new proxy.

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`ownerAddress` | string | Externally owned user account, Ethereum address that owns a GEB proxy.  |

**Returns:** *Promise‹GebProxyActions›*

___

###  getProxyActionGlobalSettlement

▸ **getProxyActionGlobalSettlement**(`ownerAddress`: string): *Promise‹GebProxyActionsGlobalSettlement›*

*Inherited from [GebAdmin](gebadmin.md).[getProxyActionGlobalSettlement](gebadmin.md#getproxyactionglobalsettlement)*

Defined in packages/geb/lib/geb.d.ts:53

Given an address returns a GebProxyActionsGlobalSettlement object to execute bundled operations during GlobalSettlement.
**IMPORTANT**: Same as for `getProxyAction` you will need to deploy a proxy beforehand using the proxy registry.

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`ownerAddress` | string | Externally owned user account, Ethereum address that owns a GEB proxy.  |

**Returns:** *Promise‹GebProxyActionsGlobalSettlement›*

___

###  getSafe

▸ **getSafe**(`idOrHandler`: string | number, `collateralType?`: string): *Promise‹Safe›*

*Inherited from [GebAdmin](gebadmin.md).[getSafe](gebadmin.md#getsafe)*

Defined in packages/geb/lib/geb.d.ts:62

Get the SAFE object

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`idOrHandler` | string &#124; number | Safe Id or SAFE handler  |
`collateralType?` | string | - |

**Returns:** *Promise‹Safe›*

___

###  multiCall

▸ **multiCall**‹**O1**, **O2**, **O3**›(`calls`: [MulticallRequest‹O1›, MulticallRequest‹O2›, MulticallRequest‹O3›]): *Promise‹[O1, O2, O3]›*

*Inherited from [GebAdmin](gebadmin.md).[multiCall](gebadmin.md#multicall)*

Defined in packages/geb/lib/geb.d.ts:86

**Type parameters:**

▪ **O1**

▪ **O2**

▪ **O3**

**Parameters:**

Name | Type |
------ | ------ |
`calls` | [MulticallRequest‹O1›, MulticallRequest‹O2›, MulticallRequest‹O3›] |

**Returns:** *Promise‹[O1, O2, O3]›*

___

###  verifyWebScheduleCallcode

▸ **verifyWebScheduleCallcode**(`govFunctionAbi`: string, `params`: any[], `earliestExecutionTime`: number, `calldata`: string): *Promise‹boolean›*

*Defined in [packages/geb-admin/src/geb-admin.ts:74](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb-admin/src/geb-admin.ts#L74)*

Verifies a transaction for scheduling proposals

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`govFunctionAbi` | string | Human readable abi from gov actions or proxy of choice -> "setDelay(address,uint256)" |
`params` | any[] | Array containing all for the above function |
`earliestExecutionTime` | number | - |
`calldata` | string | to verify |

**Returns:** *Promise‹boolean›*

Promise<TransactionRequest>

___

###  webExecuteProposal

▸ **webExecuteProposal**(`govFunctionAbi`: string, `params`: any[], `earliestExecutionTime`: number): *Promise‹TransactionRequest›*

*Defined in [packages/geb-admin/src/geb-admin.ts:96](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb-admin/src/geb-admin.ts#L96)*

Encodes executing a proposal in dspause for web GUI

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`govFunctionAbi` | string | Human readable abi from gov actions or proxy of choice -> "setDelay(address,uint256)" |
`params` | any[] | Array containing all for the above function |
`earliestExecutionTime` | number | - |

**Returns:** *Promise‹TransactionRequest›*

Promise<TransactionRequest>

___

###  webScheduleProposal

▸ **webScheduleProposal**(`govFunctionAbi`: string, `params`: any[], `earliestExecutionTime`: number): *Promise‹object›*

*Defined in [packages/geb-admin/src/geb-admin.ts:121](https://github.com/reflexer-labs/geb.js/blob/2c6df29/packages/geb-admin/src/geb-admin.ts#L121)*

Encodes scheduling a proposal in dspause for web GUI

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`govFunctionAbi` | string | Human readable abi from gov actions or proxy of choice -> "setDelay(address,uint256)" |
`params` | any[] | Array containing all for the above function |
`earliestExecutionTime` | number | - |

**Returns:** *Promise‹object›*

Promise<TransactionRequest>
