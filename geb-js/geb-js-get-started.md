# Getting Started with GEB.js

Library to interact with the GEB smart contracts. Manage your safes, mint RAI, inspect the system state, and much more.

The library is written in Typescript with full typing support. It allows access to the low level API to directly interact with the contracts.

## Install

```text
npm install geb.js
```

## Dependencies

At the moment, GEB.js requires the use of [Ether.js](https://www.npmjs.com/package/ethers) V5. In the future we will also support Web3.

```text
npm install ethers
```

## Documentation

Full API documentation is available [here](https://docs.reflexer.finance/geb-js/gettingstarted).

## Examples

This is a complete example of how you can inspect a SAFE and also open a new one using your own proxy:

```typescript
import { ethers, utils as ethersUtils } from 'ethers'
import { Geb, utils } from 'geb.js'

// Setup Ether.js
const provider = new ethers.providers.JsonRpcProvider(
    'http://kovan.infura.io/<API KEY>'
)
const wallet = new ethers.Wallet('0xdefiisawesome...', provider)

// Create the main GEB object
const geb = new Geb('kovan', provider)

// Get a SAFE
const safe = await geb.getSafe(4)
console.log(`Safe id 4 has: ${utils.wadToFixed(safe.debt).toString()} RAI of debt.`)
console.log(`It will get liquidated if ETH price falls below ${(await safe.liquidationPrice())?.toString()} USD.`)

// Open a new SAFE, lock ETH and draw RAI in a single transaction using a proxy
// Note: Before doing this you need to create your own proxy
const proxy = await geb.getProxyAction(wallet.address)
const tx = proxy.openLockETHAndGenerateDebt(
    ethersUtils.parseEther('1'), // Lock 1 Ether
    utils.ETH_A,                 // Of collateral ETH
    ethersUtils.parseEther('15') // And draw 15 RAI
)

tx.gasPrice = ethers.BigNumber.from('80').mul('1000000000') // Set the gas price to 80 Gwei
const pending = await wallet.sendTransaction(tx) // Send the transaction
console.log(`Transaction ${pending.hash} waiting to be mined...`)
await pending.wait() // Wait for it to be mined
console.log('Transaction mined, safe opened!')
```

## Additional examples

In the examples below we assume the variables are defined like in the complete example above.

1. [Deploy a New Proxy](geb-js-get-started.md#deploy-a-new-proxy)
2. [Partial Repay of Safe Debt](geb-js-get-started.md#partial-repay-of-safe-debt)
3. [Complete Repay of safe debt](geb-js-get-started.md#complete-repay-of-safe-debt)
4. [Withdraw Ether Collateral](geb-js-get-started.md#withdraw-ether-collateral)
5. [Make Direct Contract Calls](geb-js-get-started.md#make-direct-contract-calls)
6. [Multicall](geb-js-get-started.md#Multicall)

### Deploy a New Proxy

```typescript
const tx = geb.deployProxy()
await wallet.sendTransaction(tx)
```

### Partial Repay of Safe Debt

```typescript
const proxy = await geb.getProxyAction("0xdefidream...")
// Repay 1 RAI of debt to SAFE #4
const tx = proxy.repayDebt(4, ethersUtils.parseEther('1'))
const wallet.sendTransaction(tx)
```

### Complete Repayment of Safe Debt

```typescript
const proxy = await geb.getProxyAction("0xdefidream...")
// Repay all of SAFE #4's debt
const tx = proxy.repayAllDebt(4)
const wallet.sendTransaction(tx)
```

### Withdraw Ether Collateral

```typescript
const proxy = await geb.getProxyAction("0xdefidream...")
// Unlock 1 ETH of collateral from SAFE #4 and transfer it to its owner 
const tx = proxy.freeETH(4, ethersUtils.parseEther('1'))
const wallet.sendTransaction(tx)
```

### Repay all Debt and Withdraw all Collateral

```typescript
const proxy = await geb.getProxyAction("0xdefidream...")
const safe = await geb.getSafe(4)
const tx = proxy.repayAllDebtAndFreeETH(4, safe.collateral)
const wallet.sendTransaction(tx)
```

### Make Direct Contract Calls

GEB.js exposes APIs for all the core contracts in the `Geb.contracts` object. Solidity functions that are read-only \(`view` or `pure`\) asynchronously return the expected value from the chain. State changing functions will return a transaction object that is supposed to be passed by the developer to `ether.js` or `web3`.

```typescript
// Fetch system parameters from their respective contracts
const surplusBuffer = await geb.contracts.accountingEngine.surplusBuffer()
const { stabilityFee } = await geb.contracts.taxCollector.collateralTypes(utils.ETH_A)

// Liquidate a Safe
const tx = geb.contracts.liquidationEngine.liquidateSAFE(utils.ETH_A,"0xdefidream...");
await wallet.sendTransaction(tx)
```

### Multicall

Useful to bundle read-only calls in a single RPC call

```typescript
const [ globalDebt, collateralInfo ] = await geb.multiCall([
    geb.contracts.safeEngine.globalDebt(true), // !! Note the last parameter set to true.
    geb.contracts.safeEngine.collateralTypes(utils.ETH_A, true),
])
```

