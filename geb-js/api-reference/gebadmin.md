# Geb Admin

This class extends the core `GEB` class with additional tools and contracts that are not used as often as other SAFE management tools. Here you will find utils for contracts such as DSPause, ESM etc. These contracts are scattered across several repositories. Please refer to the smart contract documentation to learn more about them.

**IMPORTANT:** To avoid bloating the main [geb.js](https://www.npmjs.com/package/geb.js) package this class is only available in a [separate package](https://www.npmjs.com/package/@reflexer-finance/geb-admin). Please install it like this:

```text
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

+ **new GebAdmin**\(`network`: GebDeployment, `provider`: GebProviderInterface \| Provider\): [_GebAdmin_](gebadmin.md)

_Defined in_ [_packages/geb-admin/src/geb-admin.ts:51_](https://github.com/reflexer-labs/geb.js/blob/8c78ffc/packages/geb-admin/src/geb-admin.ts#L51)

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| `network` | GebDeployment | Either `'kovan'`, `'mainnet'` or an actual list of contract addresses. |
| `provider` | GebProviderInterface \| Provider | Either a Ethers.js provider or a GEB provider. Support for Web3.js will soon be added. |

**Returns:** [_GebAdmin_](gebadmin.md)

## Properties

### contracts

• **contracts**: _ContractApis_

_Inherited from_ [_GebAdmin_](gebadmin.md)_._[_contracts_](gebadmin.md#contracts)

Defined in packages/geb/lib/geb.d.ts:34

Object containing all GEB core contracts instances for low level interactions. All contracts object offer a one-to-one typed API to the underlying smart-contract. Currently has the following contracts:

* SAFEEngine
* AccountingEngine
* TaxCollector
* LiquidationEngine
* OracleRelayer
* GlobalSettlement
* DebtAuctionHouse
* PreSettlementSurplusAuctionHouse
* PostSettlementSurplusAuctionHouse
* SettlementSurplusAuctioneer
* GebSafeManager
* GetSafes
* BasicCollateralJoin
* CoinJoin
* Coin \(RAI ERC20 contract\)
* GebProxyRegistry
* FixedDiscountCollateralAuctionHouse \(For ETH-A\)
* Weth \(ERC20\)

### contractsAdmin

• **contractsAdmin**: _AdminApis_

_Defined in_ [_packages/geb-admin/src/geb-admin.ts:51_](https://github.com/reflexer-labs/geb.js/blob/8c78ffc/packages/geb-admin/src/geb-admin.ts#L51)

Object containing all GEB admin contracts instances for low level interactions. It currently has the following contracts:

* MultiSigWallet
* DsProxy
* DsToken
* ProtocolTokenAuthority
* GebPollingEmitter
* GebPrintingPermissions
* DsDelegateRoles
* DsPause
* DsPauseProxy
* GovActions
* ESM
* TokenBurner
* FsmGovernanceInterface
* DsProxyFactory
* GebDeployPauseProxyActions
* DsProxy
* TxManager

## Methods

### deployProxy

▸ **deployProxy**\(\): _object_

_Inherited from_ [_GebAdmin_](gebadmin.md)_._[_deployProxy_](gebadmin.md#deployproxy)

Defined in packages/geb/lib/geb.d.ts:57

Deploy a new proxy owned by the sender.

**Returns:** _object_

* **chainId**? : _number_
* **data**? : _string_
* **from**? : _string_
* **gasLimit**? : _BigNumber_
* **gasPrice**? : _BigNumber_
* **nonce**? : _number_
* **to**? : _string_
* **value**? : _BigNumber_

### getErc20Contract

▸ **getErc20Contract**\(`tokenAddress`: string\): _Erc20_

_Inherited from_ [_GebAdmin_](gebadmin.md)_._[_getErc20Contract_](gebadmin.md#geterc20contract)

Defined in packages/geb/lib/geb.d.ts:92

Returns an object that can be used to interact with a ERC20 token. Example:

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

| Name | Type | Description |
| :--- | :--- | :--- |
| `tokenAddress` | string | Token contract address |

**Returns:** _Erc20_

Erc20

### getGebContract

▸ **getGebContract**‹**T**›\(`gebContractClass`: GebContractAPIConstructorInterface‹T›, `address`: string\): _T_

_Inherited from_ [_GebAdmin_](gebadmin.md)_._[_getGebContract_](gebadmin.md#static-getgebcontract)

Defined in packages/geb/lib/geb.d.ts:126

Returns an instance of a specific geb contract given a Geb contract API class at a specified address

```typescript
import { contracts } from "geb.js"
const safeEngine = geb.getGebContract(contracts.SafeEngine, "0xabcd123..")
const globalDebt = safeEngine.globalDebt()
```

**Type parameters:**

▪ **T**: _BaseContractAPI_

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| `gebContractClass` | GebContractAPIConstructorInterface‹T› | Class from contracts or adminContracts |
| `address` | string | Contract address of the instance |

**Returns:** _T_

### getProxyAction

▸ **getProxyAction**\(`ownerAddress`: string\): _Promise‹GebProxyActions›_

_Inherited from_ [_GebAdmin_](gebadmin.md)_._[_getProxyAction_](gebadmin.md#getproxyaction)

Defined in packages/geb/lib/geb.d.ts:47

Given an address returns a GebProxyActions object to execute bundled operations. Important: This requires the address to have deployed a GEB proxy through the proxy registry contract. It will throw a `DOES_NOT_OWN_HAVE_PROXY` error if the address specified does not have a proxy. Use the [deployProxy](gebadmin.md#deployproxy) function to get a new proxy.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| `ownerAddress` | string | Externally owned user account, Ethereum address that owns a GEB proxy. |

**Returns:** _Promise‹GebProxyActions›_

### getProxyActionGlobalSettlement

▸ **getProxyActionGlobalSettlement**\(`ownerAddress`: string\): _Promise‹GebProxyActionsGlobalSettlement›_

_Inherited from_ [_GebAdmin_](gebadmin.md)_._[_getProxyActionGlobalSettlement_](gebadmin.md#getproxyactionglobalsettlement)

Defined in packages/geb/lib/geb.d.ts:53

Given an address returns a GebProxyActionsGlobalSettlement object to execute bundled operations during GlobalSettlement. **IMPORTANT**: Same as for `getProxyAction` you will need to deploy a proxy beforehand using the proxy registry.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| `ownerAddress` | string | Externally owned user account, Ethereum address that owns a GEB proxy. |

**Returns:** _Promise‹GebProxyActionsGlobalSettlement›_

### getSafe

▸ **getSafe**\(`idOrHandler`: string \| number, `collateralType?`: string\): _Promise‹Safe›_

_Inherited from_ [_GebAdmin_](gebadmin.md)_._[_getSafe_](gebadmin.md#getsafe)

Defined in packages/geb/lib/geb.d.ts:62

Get the SAFE object given a `safeManager` id or a `safeEngine` handler address.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| `idOrHandler` | string \| number | Safe Id or SAFE handler |
| `collateralType?` | string | - |

**Returns:** _Promise‹Safe›_

### getSafeFromOwner

▸ **getSafeFromOwner**\(`address`: string\): _Promise‹Safe\[\]›_

_Inherited from_ [_GebAdmin_](gebadmin.md)_._[_getSafeFromOwner_](gebadmin.md#getsafefromowner)

Defined in packages/geb/lib/geb.d.ts:73

Fetch the list of safes owned by an address. This function will fetch safes owned directly through the safeManager and safes owned through the safe manager through a proxy. Safes owned directly by the address at the safeEngine level won't appear here.

Note that this function will make a lot of network calls and is therefore very slow. For front-ends we recommend using pre-indexed data such as the geb-subgraph.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| `address` | string |  |

**Returns:** _Promise‹Safe\[\]›_

### multiCall

▸ **multiCall**‹**O1**, **O2**, **O3**›\(`calls`: \[MulticallRequest‹O1›, MulticallRequest‹O2›, MulticallRequest‹O3›\]\): _Promise‹\[O1, O2, O3\]›_

_Inherited from_ [_GebAdmin_](gebadmin.md)_._[_multiCall_](gebadmin.md#multicall)

Defined in packages/geb/lib/geb.d.ts:97

**Type parameters:**

▪ **O1**

▪ **O2**

▪ **O3**

**Parameters:**

| Name | Type |
| :--- | :--- |
| `calls` | \[MulticallRequest‹O1›, MulticallRequest‹O2›, MulticallRequest‹O3›\] |

**Returns:** _Promise‹\[O1, O2, O3\]›_

### verifyWebScheduleCallcode

▸ **verifyWebScheduleCallcode**\(`govFunctionAbi`: string, `params`: any\[\], `earliestExecutionTime`: number, `calldata`: string\): _Promise‹boolean›_

_Defined in_ [_packages/geb-admin/src/geb-admin.ts:74_](https://github.com/reflexer-labs/geb.js/blob/8c78ffc/packages/geb-admin/src/geb-admin.ts#L74)

Verifies a transaction for scheduling proposals

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| `govFunctionAbi` | string | Human readable abi from gov actions or proxy of choice -&gt; "setDelay\(address,uint256\)" |
| `params` | any\[\] | Array containing all for the above function |
| `earliestExecutionTime` | number | - |
| `calldata` | string | to verify |

**Returns:** _Promise‹boolean›_

Promise

### webExecuteProposal

▸ **webExecuteProposal**\(`govFunctionAbi`: string, `params`: any\[\], `earliestExecutionTime`: number\): _Promise‹TransactionRequest›_

_Defined in_ [_packages/geb-admin/src/geb-admin.ts:96_](https://github.com/reflexer-labs/geb.js/blob/8c78ffc/packages/geb-admin/src/geb-admin.ts#L96)

Encodes executing a proposal in dspause for web GUI

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| `govFunctionAbi` | string | Human readable abi from gov actions or proxy of choice -&gt; "setDelay\(address,uint256\)" |
| `params` | any\[\] | Array containing all for the above function |
| `earliestExecutionTime` | number | - |

**Returns:** _Promise‹TransactionRequest›_

Promise

### webScheduleProposal

▸ **webScheduleProposal**\(`govFunctionAbi`: string, `params`: any\[\], `earliestExecutionTime`: number, `description?`: string\): _Promise‹object›_

_Defined in_ [_packages/geb-admin/src/geb-admin.ts:121_](https://github.com/reflexer-labs/geb.js/blob/8c78ffc/packages/geb-admin/src/geb-admin.ts#L121)

Encodes scheduling a proposal in dspause for web GUI

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| `govFunctionAbi` | string | Human readable abi from gov actions or proxy of choice -&gt; "setDelay\(address,uint256\)" |
| `params` | any\[\] | Array containing all for the above function |
| `earliestExecutionTime` | number | - |
| `description?` | string | - |

**Returns:** _Promise‹object›_

Promise

### `Static` getGebContract

▸ **getGebContract**‹**T**›\(`gebContractClass`: GebContractAPIConstructorInterface‹T›, `address`: string, `provider`: GebProviderInterface \| Provider\): _T_

_Inherited from_ [_GebAdmin_](gebadmin.md)_._[_getGebContract_](gebadmin.md#static-getgebcontract)

Defined in packages/geb/lib/geb.d.ts:113

Returns an instance of a specific geb contract given Geb contract API class constructor at a specified address

**Type parameters:**

▪ **T**: _BaseContractAPI_

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| `gebContractClass` | GebContractAPIConstructorInterface‹T› | Class from contracts or adminContracts |
| `address` | string | Contract address of the instance |
| `provider` | GebProviderInterface \| Provider | Either a Ethers.js provider or a Geb provider |

**Returns:** _T_

