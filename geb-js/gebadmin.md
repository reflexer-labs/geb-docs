# Class: GebAdmin

This class extends the `Geb` class with additional tools and contracts that are not considered core to the system. Core contracts are mostly the contracts located in the [`geb`](https://github.com/reflexer-labs/geb) repo.
Here you will find all remaining contracts of the system such as OSM, Governance, Pause, etc.. These contracts are scattered across several repositories. Please refer to the smart contract documentation to learn how to use them.

**Important:** To avoid bloating the main [geb.js](https://www.npmjs.com/package/geb.js) package this class is only available in a [separated package](https://www.npmjs.com/package/@reflexer-finance/geb-admin).
Please install it like this:
```
npm install @reflexer-finance/geb-admin
```

And you are ready to use the admin class similarly to the Geb class. (Note that you don't need to install the geb.js package separately)

```typescript
import { ethers } from 'ethers'
import { GebAdmin } from "@reflexer-finance/geb-admin"

 const provider = new ethers.providers.JsonRpcProvider('http://kovan.infura.io/<API KEY>')
 const gebAdmin = new GebAdmin('kovan', provider)
```

## Constructors


\+ **new GebAdmin**(`network`: GebDeployment, `provider`: GebProviderInterface | Provider): *[GebAdmin](gebadmin.md)*

*Overrides void*

*Defined in [packages/geb-admin/src/geb-admin.ts:57](https://github.com/reflexer-labs/geb.js/blob/acffc2e/packages/geb-admin/src/geb-admin.ts#L57)*

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`network` | GebDeployment | Either `'kovan'`, `'mainnet'` or an actual list of contract address from the deployment script. |
`provider` | GebProviderInterface &#124; Provider | Either a Ethers.js provider or a Geb provider (Soon support for Web3 will be added)  |

**Returns:** *[GebAdmin](gebadmin.md)*

## Properties

###  contracts

• **contracts**: *ContractApis*

*Inherited from [GebAdmin](gebadmin.md).[contracts](gebadmin.md#contracts)*

Defined in packages/geb/lib/geb.d.ts:33

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

___

###  contractsAdmin

• **contractsAdmin**: *AdminApis*

*Defined in [packages/geb-admin/src/geb-admin.ts:57](https://github.com/reflexer-labs/geb.js/blob/acffc2e/packages/geb-admin/src/geb-admin.ts#L57)*

Object containing all GEB admin contracts instances for low level interactions.
Currently has the following contracts:
- Weth9
- MultiSigWallet
- DsProxy
- GebDeploy
- DsToken
- ProtocolTokenAuthority
- GebPollingEmitter
- GebPrintingPermissions
- DsRecursiveRoles
- DsPause
- DsPauseProxy
- GovActions
- Esm
- TokenBurner
- GebProxyActions
- GebProxyActionsGlobalSettlement
- FsmGovernanceInterface
- DsProxyFactory
- ChainlinkMedianEthusd or DsValue
- Osm or DsValue
- GebDeployPauseProxyActions
- DsProxy
- TxManager

## Methods

###  deployProxy

▸ **deployProxy**(): *object*

*Inherited from [GebAdmin](gebadmin.md).[deployProxy](gebadmin.md#deployproxy)*

Defined in packages/geb/lib/geb.d.ts:50

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

Defined in packages/geb/lib/geb.d.ts:74

Returns an object that can be used to interact with a ERC20 token.
Example:
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

Name | Type | Description |
------ | ------ | ------ |
`tokenAddress` | string | Token contract address |

**Returns:** *Erc20*

Erc20

___

###  getProxyAction

▸ **getProxyAction**(`ownerAddress`: string): *Promise‹GebProxyActions›*

*Inherited from [GebAdmin](gebadmin.md).[getProxyAction](gebadmin.md#getproxyaction)*

Defined in packages/geb/lib/geb.d.ts:46

Given an address returns a GebProxyAction object to execute bundled operations.
Important: This requires the address to have deployed a GEB proxy through the proxy registry contract. It will throw a `DOES_NOT_OWN_HAVE_PROXY` error if the address specified does not have a proxy. Use the [deployProxy](gebadmin.md#deployproxy) function to get a new proxy.

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`ownerAddress` | string | Externally owned user account, Ethereum address that owns a GEB proxy.  |

**Returns:** *Promise‹GebProxyActions›*

___

###  getSafe

▸ **getSafe**(`idOrHandler`: string | number): *Promise‹Safe›*

*Inherited from [GebAdmin](gebadmin.md).[getSafe](gebadmin.md#getsafe)*

Defined in packages/geb/lib/geb.d.ts:55

Get the safe object

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`idOrHandler` | string &#124; number | Safe Id or Safe handler  |

**Returns:** *Promise‹Safe›*

___

###  multiCall

▸ **multiCall**‹**O1**, **O2**, **O3**›(`calls`: [MulticallRequest‹O1›, MulticallRequest‹O2›, MulticallRequest‹O3›]): *Promise‹[O1, O2, O3]›*

*Inherited from [GebAdmin](gebadmin.md).[multiCall](gebadmin.md#multicall)*

Defined in packages/geb/lib/geb.d.ts:79

**Type parameters:**

▪ **O1**

▪ **O2**

▪ **O3**

**Parameters:**

Name | Type |
------ | ------ |
`calls` | [MulticallRequest‹O1›, MulticallRequest‹O2›, MulticallRequest‹O3›] |

**Returns:** *Promise‹[O1, O2, O3]›*
