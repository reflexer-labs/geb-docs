# Class: Geb

Main object of the library instantiating all useful GEB contracts and providing all helper functions needed.

## Constructors


\+ **new Geb**(`network`: GebDeployment, `provider`: GebProviderInterface | Provider): *[Geb](geb.md)*

*Defined in [packages/geb/src/geb.ts:45](https://github.com/reflexer-labs/geb.js/blob/bc39686/packages/geb/src/geb.ts#L45)*

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`network` | GebDeployment | Either `'kovan'`, `'mainnet'` or an actual list of contract address from the deployment script. |
`provider` | GebProviderInterface &#124; Provider | Either a Ethers.js provider or a Geb provider (Soon support for Web3 will be added)  |

**Returns:** *[Geb](geb.md)*

## Properties

###  contracts

• **contracts**: *ContractApis*

*Defined in [packages/geb/src/geb.ts:43](https://github.com/reflexer-labs/geb.js/blob/bc39686/packages/geb/src/geb.ts#L43)*

Object containing all GEB contracts instances for low level interactions. All contracts object offer a one-to-one typed API to the underlying smart-contract.
Currently has the following contracts:
- SafeEngine
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

## Methods

###  deployProxy

▸ **deployProxy**(): *TransactionRequest*

*Defined in [packages/geb/src/geb.ts:88](https://github.com/reflexer-labs/geb.js/blob/bc39686/packages/geb/src/geb.ts#L88)*

Deploy a new proxy owned by the sender.

**Returns:** *TransactionRequest*

___

###  getProxyAction

▸ **getProxyAction**(`ownerAddress`: string): *Promise‹[GebProxyActions](gebproxyactions.md)‹››*

*Defined in [packages/geb/src/geb.ts:76](https://github.com/reflexer-labs/geb.js/blob/bc39686/packages/geb/src/geb.ts#L76)*

Given an address returns a GebProxyAction object to execute bundled operations.
Important: This requires the address to have deployed a GEB proxy through the proxy registry contract. It will throw a `DOES_NOT_OWN_HAVE_PROXY` error if the address specified does not have a proxy. Use the [deployProxy](geb.md#deployproxy) function to get a new proxy.

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`ownerAddress` | string | Externally owned user account, Ethereum address that owns a GEB proxy.  |

**Returns:** *Promise‹[GebProxyActions](gebproxyactions.md)‹››*

___

###  getSafe

▸ **getSafe**(`idOrHandler`: string | number): *Promise‹[Safe](safe.md)‹››*

*Defined in [packages/geb/src/geb.ts:96](https://github.com/reflexer-labs/geb.js/blob/bc39686/packages/geb/src/geb.ts#L96)*

Get the safe object

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`idOrHandler` | string &#124; number | Safe Id or Safe handler  |

**Returns:** *Promise‹[Safe](safe.md)‹››*

___

###  multiCall

▸ **multiCall**‹**O1**, **O2**, **O3**›(`calls`: [MulticallRequest‹O1›, MulticallRequest‹O2›, MulticallRequest‹O3›]): *Promise‹[O1, O2, O3]›*

*Defined in [packages/geb/src/geb.ts:136](https://github.com/reflexer-labs/geb.js/blob/bc39686/packages/geb/src/geb.ts#L136)*

Bundles several read only GEB contract call into 1 RPC single request. Useful for front-ends or apps that need to fetch many parameters from the contracts but want to minimize the network request and the load on the underlying Ethereum node.
The function takes as input an Array of GEB view contract calls.
IMPORTANT: You have to set the `multicall` parameter of the contract function to `true`, it is the always the last parameter of the function.
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
`calls` | [MulticallRequest‹O1›, MulticallRequest‹O2›, MulticallRequest‹O3›] | Return value from a view GEB contract call. !! The GEB contract object needs to be call with the parameter `multicall` set to `true`, see example above. |

**Returns:** *Promise‹[O1, O2, O3]›*

Promise<T[]> Array with the result from their respective requests.
