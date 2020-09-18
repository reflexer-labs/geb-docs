---
description: Helper functions to interact with GEB governance related contracts
---

# Admin Utilities

This class extends the core `Geb` class with additional tools and contracts that encompass the . Core contracts are mostly the contracts located in the [`geb`](https://github.com/reflexer-labs/geb) repo. Here you will find all remaining contracts of the system such as OSM, Governance, Pause, etc.. These contracts are scattered across several repositories. Please refer to the smart contract documentation to learn how to use them.

**Important:** To avoid bloating the main [geb.js](https://www.npmjs.com/package/geb.js) package this class is only available in a [separated package](https://www.npmjs.com/package/@reflexer-finance/geb-admin). Please install it like this:

```text
npm install @reflexer-finance/geb-admin
```

And you are ready to use the admin class similarly to the Geb class. \(Note that you don't need to install the geb.js package separately\)

```typescript
import { ethers } from 'ethers'
import { GebAdmin } from "@reflexer-finance/geb-admin"

 const provider = new ethers.providers.JsonRpcProvider('http://kovan.infura.io/<API KEY>')
 const gebAdmin = new GebAdmin('kovan', provider)
```

## Constructors

+ **new GebAdmin**\(`network`: GebDeployment, `provider`: GebProviderInterface \| Provider\): [_GebAdmin_](admin-utilities.md)

_Defined in_ [_packages/geb-admin/src/geb-admin.ts:57_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb-admin/src/geb-admin.ts#L57)

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| `network` | GebDeployment | Either `'kovan'`, `'mainnet'` or an actual list of contract address from the deployment script. |
| `provider` | GebProviderInterface \| Provider | Either a Ethers.js provider or a Geb provider \(Soon support for Web3 will be added\) |

**Returns:** [_GebAdmin_](admin-utilities.md)

## Properties

### contracts

• **contracts**: _ContractApis_

_Inherited from_ [_GebAdmin_](admin-utilities.md)_._[_contracts_](admin-utilities.md#contracts)

Defined in packages/geb/lib/geb.d.ts:34

Object containing all GEB core contracts instances for low level interactions. All contracts object offer a one-to-one typed API to the underlying smart-contract. Currently has the following contracts:

* SafeEngine
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

_Defined in_ [_packages/geb-admin/src/geb-admin.ts:57_](https://github.com/reflexer-labs/geb.js/blob/31f836f/packages/geb-admin/src/geb-admin.ts#L57)

Object containing all GEB admin contracts instances for low level interactions. Currently has the following contracts:

* Weth9
* MultiSigWallet
* DsProxy
* GebDeploy
* DsToken
* ProtocolTokenAuthority
* GebPollingEmitter
* GebPrintingPermissions
* DsRecursiveRoles
* DsPause
* DsPauseProxy
* GovActions
* Esm
* TokenBurner
* GebProxyActions
* GebProxyActionsGlobalSettlement
* FsmGovernanceInterface
* DsProxyFactory
* ChainlinkMedianEthusd or DsValue
* Osm or DsValue
* GebDeployPauseProxyActions
* DsProxy
* TxManager

## Methods

### deployProxy

▸ **deployProxy**\(\): _object_

_Inherited from_ [_GebAdmin_](admin-utilities.md)_._[_deployProxy_](admin-utilities.md#deployproxy)

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

_Inherited from_ [_GebAdmin_](admin-utilities.md)_._[_getErc20Contract_](admin-utilities.md#geterc20contract)

Defined in packages/geb/lib/geb.d.ts:81

Returns an object that can be used to interact with a ERC20 token. Example:

```typescript
const USDCAddress = "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48"
const USDC = geb.getErc20Contract(USDCAddress)

// Get deadbeef's balance
const balance = USDC.balanceOf("0xdeadbeef..")

// Send 1 USDC to deadbeef (Yes, USDC is 6 decimals)
const tx = USDC.transfer("0xdeadbeef..", "1000000")
await wallet.sendTransaction(tx)
```

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| `tokenAddress` | string | Token contract address |

**Returns:** _Erc20_

Erc20

### getProxyAction

▸ **getProxyAction**\(`ownerAddress`: string\): _Promise‹GebProxyActions›_

_Inherited from_ [_GebAdmin_](admin-utilities.md)_._[_getProxyAction_](admin-utilities.md#getproxyaction)

Defined in packages/geb/lib/geb.d.ts:47

Given an address returns a GebProxyAction object to execute bundled operations. Important: This requires the address to have deployed a GEB proxy through the proxy registry contract. It will throw a `DOES_NOT_OWN_HAVE_PROXY` error if the address specified does not have a proxy. Use the [deployProxy](admin-utilities.md#deployproxy) function to get a new proxy.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| `ownerAddress` | string | Externally owned user account, Ethereum address that owns a GEB proxy. |

**Returns:** _Promise‹GebProxyActions›_

### getProxyActionGlobalSettlement

▸ **getProxyActionGlobalSettlement**\(`ownerAddress`: string\): _Promise‹GebProxyActionsGlobalSettlement›_

_Inherited from_ [_GebAdmin_](admin-utilities.md)_._[_getProxyActionGlobalSettlement_](admin-utilities.md#getproxyactionglobalsettlement)

Defined in packages/geb/lib/geb.d.ts:53

Given an address returns a GebProxyActionsGlobalSettlement object to execute bundled operations during GlobalSettlement. Important: Same as for `getProxyAction` it requires a proxy deploy through the registry.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| `ownerAddress` | string | Externally owned user account, Ethereum address that owns a GEB proxy. |

**Returns:** _Promise‹GebProxyActionsGlobalSettlement›_

### getSafe

▸ **getSafe**\(`idOrHandler`: string \| number\): _Promise‹Safe›_

_Inherited from_ [_GebAdmin_](admin-utilities.md)_._[_getSafe_](admin-utilities.md#getsafe)

Defined in packages/geb/lib/geb.d.ts:62

Get the safe object

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| `idOrHandler` | string \| number | Safe Id or Safe handler |

**Returns:** _Promise‹Safe›_

### multiCall

▸ **multiCall**‹**O1**, **O2**, **O3**›\(`calls`: \[MulticallRequest‹O1›, MulticallRequest‹O2›, MulticallRequest‹O3›\]\): _Promise‹\[O1, O2, O3\]›_

_Inherited from_ [_GebAdmin_](admin-utilities.md)_._[_multiCall_](admin-utilities.md#multicall)

Defined in packages/geb/lib/geb.d.ts:86

**Type parameters:**

▪ **O1**

▪ **O2**

▪ **O3**

**Parameters:**

| Name | Type |
| :--- | :--- |
| `calls` | \[MulticallRequest‹O1›, MulticallRequest‹O2›, MulticallRequest‹O3›\] |

**Returns:** _Promise‹\[O1, O2, O3\]›_

