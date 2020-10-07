# Global Settlement

The following are examples of how you can use GEB.js to facilitate the Global Settlement process and redeem your collateral that is locked in Safes or directly exchange system coins with collateral.

## Example Flow for Global Settlement Using GEB.js

This code implements the steps described in the [Global Settlement page](https://docs.reflexer.finance/system-contracts/shutdown-module/global-settlement#the-shutdown-mechanism-9-crucial-steps). 

We first need to setup geb.js and ethers. We will use these variables in the following steps of the procedure.

```typescript
import { ethers } from 'ethers'
import { Geb, utils } from 'geb.js'

const provider = new ethers.providers.JsonRpcProvider(
    'http://kovan.infura.io/<API KEY>'
)
const wallet = new ethers.Wallet('0xdefiisawesome...', provider)
const geb = new Geb('kovan', provider)

```

Before continuing, we need to make sure that the global settlement was triggered by checking the shutdown timestamp: 

```typescript
const shutdownTime = await geb.contracts.globalSettlement.shutdownTime()
const hasGlobalSettlementStarted = shutdownTime.gt(0)
```

### Withdraw excess collateral

After the global settlement started, each collateral need to be frozen individually \([Step 2](https://docs.reflexer.finance/system-contracts/shutdown-module/global-settlement#2-cage-ilk)\). This needs to be done only once for each collateral type. 

```typescript
const tx = geb.contracts.globalSettlement.freezeCollateralType(utils.ETH_A)
await wallet.sendTransaction(tx)
```

Since SAFEs are supposed to be over-collateralize its owner can already withdraw the excess collateral. This following code assume the SAFE is owned by a proxy and uses the [Global Settlement Proxy](https://docs.reflexer.finance/geb-js/geb-js-global-settlement-proxies).

```typescript
const gsProxy = await geb.getProxyActionGlobalSettlement(wallet.address)
const wethJoinAddress = geb.contracts.joinETH_A.address
// Withdraw excess collateral from safe id 3
const tx = gsProxy.freeTokenCollateral(wethJoinAddress, 3)
await wallet.sendTransaction(tx)
```

This fulfills the [step 3](https://docs.reflexer.finance/system-contracts/shutdown-module/global-settlement#3-skim-ilk-urn) for your own safe and [step 5](https://docs.reflexer.finance/system-contracts/shutdown-module/global-settlement#5-free-ilk).  

### Settled final RAI exchange rates 

This part of the process consists in determining a price to exchange the RAIs still in circulation for collateral. The system needs to account for all SAFEs \(I\), terminate all ongoing collateral auctions \(II\) and sell the system surplus \(III\).

This needs to be done only once for the whole system. These steps can be taken care of by the [settlement keeper](https://github.com/reflexer-labs/settlement-keeper) bot.

```typescript
// For (I) the function `processSAFE` needs to be manually called all SAFEs.
// Particularly for under-collateralized SAFEs since their owners are not 
// incentivised to call it themself.
const safes = [] // Gather some safe handlers 
safes.push((await geb.getSafe(2)).handler)
safes.push((await geb.getSafe(3)).handler)
safes.push((await geb.getSafe(4)).handler)
// ...
// Prepare the transactions
const txs = safes.map(handler => 
    geb.contracts.globalSettlement.processSAFE(utils.ETH_A, handler)
)
// Send all transactions
txs.map(tx => await wallet.sendTransaction(tx))


// For (II) we have 2 scenarios. Either wait for all auctions to finish by 
// waiting the cooldown:
const cooldown = await geb.contracts.globalSettlement.shutdownCooldown()
const now = BigNumber.from(Date.now()).div(1000)
const isCooldownPassed = now.gt(shutdownTime.add(cooldown))
// Or prematurely terminate each auction:
const auctionId = 6
const tx = geb.contracts.globalSettlement.fastTrackAuction(utils.ETH_A, auctionId)
await wallet.sendTransaction(tx)

// (III) We need to get rid of the system surplus.
const tx = geb.contracts.accountingEngine.transferPostSettlementSurplus()
await wallet.sendTransaction(tx)

```

Finally, the cash prices for each collateral can be set with the 2 following steps:

```typescript
const tx = geb.contracts.globalSettlement.setOutstandingCoinSupply()
await wallet.sendTransaction(tx)

// To be called once for each collateral type
const tx = geb.contracts.globalSettlement.calculateCashPrice(utils.ETH_A)
await wallet.sendTransaction(tx)
```

### Redeem collateral against RAI

At this stage, any RAI holder can exchange RAI against a fixed basket of collateral. It is a 2 step process that consists in first locking RAI and then claim a share of each collateral. For the task, we use a GEB proxy with [Global Settlement Proxies](https://docs.reflexer.finance/geb-js/geb-js-global-settlement-proxies).

```typescript
// Lock the totality of the RAI balance
const raiBalance = await geb.contracts.coin.balanceOf(wallet.address)
const tx = gsProxy.prepareCoinsForRedeeming(raiBalance)
await wallet.sendTransaction(tx)

// This needs to be called once for each collateral type
const tx = gsProxy.redeemTokenCollateral(wethJoinAddress, utils.ETH_A, raiBalance)
await wallet.sendTransaction(tx)
```

