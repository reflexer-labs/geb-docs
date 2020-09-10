# GEB.js

Library to interact with the GEB smart contracts. Manage your safes, mint RAI, inspect the system state, and much more.

The library is written in Typescript with full typing support. It allows access to the low level API to directly interact with the contracts.

## Install

```
npm install geb.js
```

## Dependencies

At the moment, Geb.js requires to use [Ether.js](https://www.npmjs.com/package/ethers) V5. In the future we will support Web3.

```
npm install ethers
```

## Documentation

Full API documentation is available [here](https://docs.reflexer.finance/geb-js/gettingstarted).

## Examples

How to inspect your safe and open a new safe with a proxy:
```typescript
import { ethers, utils as ethersUtils } from 'ethers'
import { Geb, utils } from 'geb.js'

// Setup Ether.js
const provider = new ethers.providers.JsonRpcProvider(
    'http://kovan.infura.io/<API KEY>'
)
const wallet = new ethers.Wallet('0xdeadbeef...', provider)

// Create the main GEB object
const geb = new Geb('kovan', provider)

// Get a safe
const safe = await geb.getSafe(4)
console.log(`Safe id 4 has: ${utils.wadToFixed(safe.debt).toString()} RAI of debt.`)
console.log(`It will get liquidated if ETH price falls below ${(await safe.liquidationPrice())?.toString()} USD.`)

// Open a new safe, lock ETH and draw RAI in a single transaction with a proxy
// Note: Before doing this you need a proxy
const proxy = await geb.getProxyAction(wallet.address)
const tx = await proxy.openLockETHAndGenerateDebt(
    ethersUtils.parseEther('1'), // Lock 1 Ether
    utils.ETH_A, // Of collateral ETH
    ethersUtils.parseEther('15') // Draw 15 RAI
)

tx.gasPrice = ethers.BigNumber.from('80').mul('1000000000') // Set the gas price to 80 Gwei
const pending = await wallet.sendTransaction(tx) // Send the transaction
console.log(`Transaction ${pending.hash} waiting to be mined...`)
await pending.wait() // Wait for it to be mined
console.log('Transaction mined, safe opened!')
```

Deploy a GEB proxy to use proxy action:
```typescript
const tx = geb.deployProxy()
wallet.sendTransaction(tx)
```

Use the low level API to make direct contract calls:
```typescript
// Fetch some system parameters from their respective contracts
const surplusBuffer = await geb.contracts.accountingEngine.surplusBuffer()
const { stabilityFee } = await geb.contracts.taxCollector.collateralTypes(utils.ETH_A)

// Liquidate a Safe
const tx = geb.contracts.liquidationEngine.liquidateSAFE(utils.ETH_A,"0xdeadbeef..");
await wallet.sendTransaction(tx)
```

Use multicall to bundle RPC requests into one reducing the network traffic:

```typescript
const [ globalDebt, collateralInfo ] = await geb.multiCall([
    geb.contracts.safeEngine.globalDebt(true), // !! Note the last parameter set to true.
    geb.contracts.safeEngine.collateralTypes(utils.ETH_A, true),
])
```
