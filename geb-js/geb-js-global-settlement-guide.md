# Global Settlement Guide

The following are examples of how you can use GEB.js to facilitate the Global Settlement process and redeem your collateral which is locked in your own Safes or directly exchange system coins with collateral.

## Example Flow for Global Settlement Using GEB.js

These scripts can help you go throught the steps described on the [Global Settlement](https://docs.reflexer.finance/system-contracts/shutdown-module/global-settlement#the-shutdown-mechanism-9-crucial-steps) page. 

We first need to setup `geb.js` and `ethers.js`:

```typescript
import { ethers } from 'ethers'
import { Geb, utils } from 'geb.js'

const provider = new ethers.providers.JsonRpcProvider(
    'http://kovan.infura.io/v3/<API KEY>'
)
const wallet = new ethers.Wallet('0xdefiisawesome...', provider)
const geb = new Geb('kovan', provider)
```

Before continuing, we need to make sure that Global Settlement was triggered by checking the shutdown timestamp: 

```typescript
const shutdownTime = await geb.contracts.globalSettlement.shutdownTime()
const hasGlobalSettlementStarted = shutdownTime.gt(0)
```

### Withdraw Excess Collateral

After settlement starts, each collateral needs to be frozen \([Step 2](https://docs.reflexer.finance/system-contracts/shutdown-module/global-settlement#2-cage-ilk)\). This needs to be done only once for every collateral type. 

```typescript
const tx = geb.contracts.globalSettlement.freezeCollateralType(utils.ETH_A)
await wallet.sendTransaction(tx)
```

Since a SAFE is supposed to be over-collateralized, its owner can already withdraw excess collateral. The following script assumes that the SAFE is owned by a [proxy](https://github.com/reflexer-labs/ds-proxy/blob/master/src/proxy.sol) contract. It also uses the Global Settlement Proxy Actions to pack and atomically execute multiple transactions at once.

```typescript
const proxy = await geb.getProxyAction(wallet.address)
const wethJoinAddress = geb.contracts.joinETH_A.address
// Withdraw excess collateral from the Safe with ID #3
const tx = proxy.freeTokenCollateralGlobalSettlement(wethJoinAddress, 3)
await wallet.sendTransaction(tx)
```

This fulfills [step 3](https://docs.reflexer.finance/system-contracts/shutdown-module/global-settlement#3-skim-ilk-urn) and [step 5](https://docs.reflexer.finance/system-contracts/shutdown-module/global-settlement#5-free-ilk) from the Global Settlement process.

### Set the Final COL/COIN Exchange Rates 

This part of the process consists in determining an exchange rate between the system coins that are still in circulation and each individual collateral type accepted by the system. The system needs to account for all Safes \(I\), terminate all ongoing collateral auctions \(II\) and remove all system surplus \(III\).

This needs to be done only once for the whole system. These steps can be taken care of by the [settlement keeper](https://github.com/reflexer-labs/settlement-keeper) bot or by anyone who is willing to pay the gas costs associated with these transactions.

```typescript
// For (I) the function `processSAFE` needs to be called for all SAFEs,
// particularly for under-collateralized ones since their owners are not 
// incentivised to call it themselves
const safes = [] // Gather some Safe handlers 
safes.push((await geb.getSafe(2)).handler)
safes.push((await geb.getSafe(3)).handler)
safes.push((await geb.getSafe(4)).handler)

// Prepare the transactions
const txs = safes.map(handler => 
    geb.contracts.globalSettlement.processSAFE(utils.ETH_A, handler)
)

// Send all transactions
txs.map(tx => await wallet.sendTransaction(tx))

// For (II) we have 2 possibilities: wait for all auctions to finish
const cooldown = await geb.contracts.globalSettlement.shutdownCooldown()
const now = BigNumber.from(Date.now()).div(1000)
const isCooldownPassed = now.gt(shutdownTime.add(cooldown))

// Or prematurely terminate each auction
const auctionId = 6
const tx = geb.contracts.globalSettlement.fastTrackAuction(utils.ETH_A, auctionId)
await wallet.sendTransaction(tx)

// (III) We need to get rid of the system surplus
const accountingEngineAddress = geb.contracts.accountingEngine.address
const coin = await  geb.contracts.safeEngine.coinBalance(accountingEngineAddress)
const debt = await geb.contracts.safeEngine.debtBalance(accountingEngineAddress)
const amountToSettle = coin.gte(debt) ? debt : coin
const tx = geb.contracts.accountingEngine.settleDebt(amountToSettle)
await wallet.sendTransaction(tx)

// In case there is a bug in the system's accounting that 
// created more surplus than debt, there is a backup function called
// transferPostSettlementSurplus() which gets rid of that extra surplus
// and allows GlobalSettlement.setOutstandingCoinSupply() to execute successfuly
const tx = geb.contracts.accountingEngine.transferPostSettlementSurplus()
await wallet.sendTransaction(tx)
```

Finally, the cash price for each collateral can be set with the following steps:

```typescript
const tx = geb.contracts.globalSettlement.setOutstandingCoinSupply()
await wallet.sendTransaction(tx)

// To be called once for each collateral type
const tx = geb.contracts.globalSettlement.calculateCashPrice(utils.ETH_A)
await wallet.sendTransaction(tx)
```

### Redeem Collateral against System Coins

At this stage, any system coin holder can exchange their coins against a fixed basket of collateral. This is a 2 step process that consists in locking and preparing system coins and then claiming a share of a specific collateral type.

```typescript
// Prepare the system coins
const systemCoinBalance = await geb.contracts.coin.balanceOf(wallet.address)
const tx = proxy.prepareCoinsForRedeemingGlobalSettlement(systemCoinBalance)
await wallet.sendTransaction(tx)

// Redeem any collateral type you want
const tx = proxy.redeemTokenCollateralGlobalSettlement(wethJoinAddress, utils.ETH_A, systemCoinBalance)
await wallet.sendTransaction(tx)
```

