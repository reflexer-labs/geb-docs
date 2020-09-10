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

## Getting stated

Example:

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
console.log(
    `Safe id 4 has: ${utils.wadToFixed(safe.debt).toString()} RAI of debt`
)

// Open a new safe, lock ETH and draw RAI in a single transaction with a proxy
// Note: You need a proxy prior to doing that
const proxy = await geb.getProxyAction(wallet.address)
const tx = await proxy.openLockETHAndGenerateDebt(
    ethersUtils.parseEther('1'), // Lock 1 Ether
    ETH_A, // Of collateral ETH
    ethersUtils.parseEther('15') // Draw 15 RAI
)

tx.gasPrice = ethers.BigNumber.from('80').mul('1000000000') // Set the gas price to 80 Gwei
const pending = await wallet.sendTransaction(tx) // Send the transaction
console.log(`Transaction ${pending.hash} waiting to be mined...`)
await pending.wait() // Wait for it to be mined
console.log('Transaction mined, safe opened!')
```
