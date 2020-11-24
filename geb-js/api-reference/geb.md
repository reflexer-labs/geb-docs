# Class: Geb

The main package used to interact with the GEB system. Includes [helper functions](geb.md#deployproxy) for safe
 management and the [contract interface object](geb.md#contracts) to directly call the smart-contracts.

## Constructors


\+ **new Geb**(`network`: GebDeployment, `provider`: GebProviderInterface | Provider): *[Geb](geb.md)*

*Defined in [packages/geb/src/geb.ts:88](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/geb.ts#L88)*

Constructor of the main Geb.js object.

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`network` | GebDeployment | Either `'kovan'`, `'mainnet'` or an actual list of contract addresses. |
`provider` | GebProviderInterface &#124; Provider | Either a Ethers.js provider or a Geb provider (Soon support for Web3 will be added)  |

**Returns:** *[Geb](geb.md)*

## Properties

###  contracts

• **contracts**: *ContractApis*

*Defined in [packages/geb/src/geb.ts:86](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/geb.ts#L86)*

Object containing all GEB core smart-contracts instances for direct level interactions. All of the
following contracts object are one-to-one typed API to the underlying smart-contract. Read-only
functions that do not change the blockchain state return a promise of the return data. State modifying
function will return synchronously a pre-filled transaction request object:
```
{
  to: "0x123abc.."
  data: "0xabab234ab..."
}
```
This object follow the [TransactionRequest model of ethers.js](https://docs.ethers.io/v5/api/providers/types/#providers-TransactionRequest) (Also similar to the
[model used by web.js](https://web3js.readthedocs.io/en/v1.3.0/web3-eth.html#id84)). The object can
be completed with properties such as ` from `, ` gasPrice `, ` gas ` (gas limit, web3.js ony) or
` gasLimit ` (gas limit, ethers.js only). The object can then be passed to the `sendTransaction` of
[ehters.js](https://docs.ethers.io/v5/api/signer/#Signer-sendTransaction) or
[web3.js](https://web3js.readthedocs.io/en/v1.3.0/web3-eth.html#sendtransaction)

 Example:
 ```typescript
 // Setup geb.js an ethers
 const provider = new ethers.providers.JsonRpcProvider('http://kovan.infura.io/<API KEY>')
 const wallet = new ethers.Wallet('<Private key>', provider)
 const geb = new Geb('kovan', provider)

 // Contract read function: Fetch the debt ceiling
 const debtCeiling = await geb.contracts.safeEngine.globalDebtCeiling()

 // State changing function: Manualy liquidate a SAFE
 const tx = geb.contracts.liquidationEngine.liquidateSAFE(ETH_A, '0x1234abc...')
 await wallet.sendTransaction(tx) // Send the Ethereum transaction
 ```

Currently the following contracts ae available in this property:
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

For detailed information about the functions of each contract we recommend referring directly to the
smart-contract [code](https://github.com/reflexer-labs/geb) and [documentation](https://docs.reflexer.finance/)

## Methods

###  deployProxy

▸ **deployProxy**(): *TransactionRequest*

*Defined in [packages/geb/src/geb.ts:132](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/geb.ts#L132)*

Deploy a new proxy owned by the sender.

**Returns:** *TransactionRequest*

___

###  getErc20Contract

▸ **getErc20Contract**(`tokenAddress`: string): *Erc20*

*Defined in [packages/geb/src/geb.ts:260](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/geb.ts#L260)*

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

###  getGebContract

▸ **getGebContract**‹**T**›(`gebContractClass`: GebContractAPIConstructorInterface‹T›, `address`: string): *T*

*Defined in [packages/geb/src/geb.ts:367](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/geb.ts#L367)*

Returns an instance of a specific geb contract given a Geb contract API class at a specified address

```typescript
import { contracts } from "geb.js"
const safeEngine = geb.getGebContract(contracts.SafeEngine, "0xabcd123..")
const globalDebt = safeEngine.globalDebt()
```

**Type parameters:**

▪ **T**: *BaseContractAPI*

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`gebContractClass` | GebContractAPIConstructorInterface‹T› | Class from contracts or adminContracts |
`address` | string | Contract address of the instance  |

**Returns:** *T*

___

###  getProxyAction

▸ **getProxyAction**(`ownerAddress`: string): *Promise‹[GebProxyActions](gebproxyactions.md)‹››*

*Defined in [packages/geb/src/geb.ts:120](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/geb.ts#L120)*

Given an address returns a GebProxyActions object to execute bundled operations.
Important: This requires the address to have deployed a GEB proxy through the proxy registry contract. It will throw a `DOES_NOT_OWN_HAVE_PROXY` error if the address specified does not have a proxy. Use the [deployProxy](geb.md#deployproxy) function to get a new proxy.

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`ownerAddress` | string | Externally owned user account, Ethereum address that owns a GEB proxy.  |

**Returns:** *Promise‹[GebProxyActions](gebproxyactions.md)‹››*

___

###  getSafe

▸ **getSafe**(`idOrHandler`: string | number, `collateralType?`: string): *Promise‹[Safe](safe.md)›*

*Defined in [packages/geb/src/geb.ts:140](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/geb.ts#L140)*

Get the SAFE object given a `safeManager` id or a `safeEngine` handler address.

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`idOrHandler` | string &#124; number | Safe Id or SAFE handler  |
`collateralType?` | string | - |

**Returns:** *Promise‹[Safe](safe.md)›*

___

###  getSafeFromOwner

▸ **getSafeFromOwner**(`address`: string): *Promise‹[Safe](safe.md)[]›*

*Defined in [packages/geb/src/geb.ts:223](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/geb.ts#L223)*

Fetch the list of safes owned by an address. This function will fetch safes owned directly
through the safeManager and safes owned through the safe manager through a proxy. Safes owned
directly by the address at the safeEngine level won't appear here.

Note that this function will make a lot of network calls and is therefore very slow. For
front-ends we recommend using pre-indexed data such as the geb-subgraph.

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`address` | string |   |

**Returns:** *Promise‹[Safe](safe.md)[]›*

___

###  multiCall

▸ **multiCall**‹**O1**, **O2**, **O3**›(`calls`: [MulticallRequest‹O1›, MulticallRequest‹O2›, MulticallRequest‹O3›]): *Promise‹[O1, O2, O3]›*

*Defined in [packages/geb/src/geb.ts:273](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/geb.ts#L273)*

Bundles several read only GEB contract call into 1 RPC single request. Useful for front-ends or apps that need to fetch many parameters from the contracts but want to minimize the network request and the load on the underlying Ethereum node.
The function takes as input an Array of GEB view contract calls.
**IMPORTANT**: You have to set the `multicall` parameter of the contract function to `true`, it is the always the last parameter of the function.
Multicall works for all contracts in the `Geb.contracts` and can be use with any contract that inherit the `BaseContractApi`. Note that it does not support non-view calls (Calls that require to pay gas and change the state of the blockchain).

Example:
```typescript
import { ethers } from "ethers"
import { Geb } from "geb.js"

const provider = new ethers.providers.JsonRpcProvider("http://kovan.infura.io/...")
const geb = new Geb("kovan", provider);

const [ globalDebt, collateralInfo ] = await geb.multiCall([
    geb.contracts.safeEngine.globalDebt(true), // !! Note the last parameter set to true.
    geb.contracts.safeEngine.collateralTypes(ETH_A, true),
])

console.log(`Current global debt: ${globalDebt.toString()}`)
console.log(`Current ETH_A debt: ${collateralInfo.debtAmount}`)
```

**Type parameters:**

▪ **O1**

▪ **O2**

▪ **O3**

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`calls` | [MulticallRequest‹O1›, MulticallRequest‹O2›, MulticallRequest‹O3›] | Call a read only GEB contract function. The GEB contract object needs to be called with the parameter `multicall` set to `true` as seen in the example above. |

**Returns:** *Promise‹[O1, O2, O3]›*

Promise<T[]> Array with the result from their respective requests.

___

### `Static` getGebContract

▸ **getGebContract**‹**T**›(`gebContractClass`: GebContractAPIConstructorInterface‹T›, `address`: string, `provider`: GebProviderInterface | Provider): *T*

*Defined in [packages/geb/src/geb.ts:335](https://github.com/reflexer-labs/geb.js/blob/12d498b/packages/geb/src/geb.ts#L335)*

Returns an instance of a specific geb contract given Geb contract API class constructor at a specified address

**Type parameters:**

▪ **T**: *BaseContractAPI*

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`gebContractClass` | GebContractAPIConstructorInterface‹T› | Class from contracts or adminContracts |
`address` | string | Contract address of the instance |
`provider` | GebProviderInterface &#124; Provider | Either a Ethers.js provider or a Geb provider  |

**Returns:** *T*
