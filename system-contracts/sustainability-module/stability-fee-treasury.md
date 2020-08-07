---
description: The protocol's invoice processor
---

# Stability Fee Treasury

## 1. Summary <a id="1-introduction-summary"></a>

The `StabilityFeeTreasury` is meant to allow other contracts or EOAs to pull funds \(stability fees\) in order to finance their operations. The treasury is set up as a `secondaryReceiver` in the `TaxCollector`. Anyone can periodically call the contract in order to recompute an optimal amount of fees that should be kept in the treasury and send any additional surplus to the `AccountingEngine`.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

* `allowance[receiver: address]` - withdrawal allowance
* `cdpEngine` - address of the `CDPEngine`
* `systemCoin` - system coin address \([Coin.sol](https://github.com/reflexer-labs/geb/blob/master/src/Coin.sol)\)
* `coinJoin` - system coin adapter address
* `accountingEngine` - address of the `AccountingEngine`
* `treasuryCapacity` - maximum amount of stability fees should be kept in the treasury
* `minimumFundsRequired` - minimum amount of stability fees that must be kept in the treasury at all times
* `expensesMultiplier` - multiplier for the expenses incurred since the last `transferSurplusFunds` call. Ensures a buffer of funds on top of the expected expenses until the next 
* `surplusTransferDelay` - delay between two `transferSurplusFunds` calls
* `expensesAccumulator` - total expenses incurred since treasury deployment
* `accumulatorTag` - latest snapshot of `expensesAccumulator`
* `latestSurplusTransferTime` - last timestamp when `transferSurplusFunds` was called
* `contractEnabled` - global settlement flag
* `modifyParameters` - authorized function for changing treasury parameters
* `disableContract` - disable the treasury and transfer all of its funds to the `accountingEngine`
* `joinAllCoins` - join any ERC20 system coins that the treasury has into internal coin form \(`CDPEngine.coinBalance`\)
* `allow(account: address, amount: uint256)` - authorized function that changes the allowed amount an address can withdraw from the treasury
* `giveFunds(account: address, rad: uint256)` - governance controlled function that transfers stability fees from the treasury to a destination address
* `takeFunds(account: address, rad: uint256)` - governance controlled function that transfers stability fees from a target address to the treasury
* `pullFunds(dstAccount: address, token: address, wad: uint256)` - 
* `transferSurplusFunds` - recalculate the amount of stability fees that should remain in the treasury and transfer additional funds to the `accountingEngine`

## 3. Walkthrough <a id="2-contract-details"></a>

The treasury is funded by stability fees coming from the `TaxCollector` or by anyone who is willing to send system coins \(either internally or as ERC20 tokens\) to it. Governance can also use `takeFunds` to transfer internal system coins from a source address to the treasury.  
  
Governance is also in charge with setting up authorized addresses that can `pullFunds` out of the treasury \(if their `allowance` permits\) as well as setting treasury parameters in order to determine the optimum amount of funds that should remain in the contract at any time. We define "optimum" as a multiplier \(`expensesMultiplier`\) of the latest expenses incurred by the treasury since the last `transferSurplusFunds` call. 

`transferSurplusFunds` is the way the treasury recalculates the amount of funds it should keep in reserves and transfers any surplus to the `AccountingEngine`. Note that there is a `surplusTransferDelay` time delay between recalculating the optimum and transferring surplus out of the contract.



