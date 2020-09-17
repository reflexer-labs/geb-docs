---
description: The protocol's invoice processor
---

# Stability Fee Treasury

## 1. Summary <a id="1-introduction-summary"></a>

The `StabilityFeeTreasury` is meant to allow other contracts or EOAs to pull funds \(stability fees\) in order to finance their operations. The treasury is set up as a `secondaryReceiver` in the `TaxCollector`. Anyone can periodically call the contract in order to recompute an optimal amount of fees that should be kept in the treasury and send any additional surplus to the `AccountingEngine`.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `allowance[receiver: address]` - total withdrawal allowance
* `pulledPerBlock[usr: address`, `blockNr: uint256]` - total amount pulled by a user on a specific block
* `safeEngine` - address of the `SAFEEngine`
* `systemCoin` - system coin address \([Coin.sol](https://github.com/reflexer-labs/geb/blob/master/src/Coin.sol)\)
* `coinJoin` - system coin adapter address
* `accountingEngine` - address of the `AccountingEngine`
* `treasuryCapacity` - maximum amount of stability fees should be kept in the treasury
* `minimumFundsRequired` - minimum amount of stability fees that must be kept in the treasury at all times
* `expensesMultiplier` - multiplier for the expenses incurred since the last `transferSurplusFunds` call. Ensures there's a buffer of funds on top of the expected expenses until the next 

  `transferSurplusFunds` call

* `surplusTransferDelay` - delay between two `transferSurplusFunds` calls
* `expensesAccumulator` - total expenses incurred since treasury deployment
* `accumulatorTag` - latest snapshot of `expensesAccumulator`
* `latestSurplusTransferTime` - last timestamp when `transferSurplusFunds` was called
* `contractEnabled` - global settlement flag

**Functions**

* `modifyParameters()` - authorized function for changing treasury parameters
* `disableContract()` - disable the treasury and transfer all of its funds to the `accountingEngine`
* `joinAllCoins()` - join any ERC20 system coins that the treasury has into internal coin form \(`SAFEEngine.coinBalance`\)
* `getAllowance(account: address)` - get the total and the per-block allowance for a user
* `setTotalAllowance(account: address`, `rad: uint256)` - authorized function that changes the total allowed amount an address can withdraw from the treasury
* `setPerBlockAllowance(account: address`, `rad: uint256)` - authorized function that changes the amount that an address can withdraw from the treasury every block
* `giveFunds(account: address`, `rad: uint256)` - governance controlled function that transfers stability fees from the treasury to a destination address
* `takeFunds(account: address`, `rad: uint256)` - governance controlled function that transfers stability fees from a target address to the treasury
* `pullFunds(dstAccount: address`, `token: address`, `wad: uint256)` - take funds from the treasury. Reverts if the caller is not allowed to withdraw or if their per-block allowance doesn't allow it
* `transferSurplusFunds()` - recalculate the amount of stability fees that should remain in the treasury and transfer additional funds to the `accountingEngine`

## 3. Walkthrough <a id="2-contract-details"></a>

The treasury is funded by stability fees coming from the `TaxCollector` or by anyone who is willing to send system coins \(either internally or as ERC20 tokens\) to it. Governance can also use `takeFunds` to transfer internal system coins from a source address to the treasury.  
  
Governance is in charge with setting up authorized addresses that can `pullFunds` out of the treasury \(if their `allowance` permits\) as well as setting treasury parameters in order to determine the optimum amount of funds that should remain in the contract at any time. We define "optimum" as a multiplier \(`expensesMultiplier`\) of the latest expenses incurred by the treasury since the last `transferSurplusFunds` call. 

`transferSurplusFunds` is the way the treasury recalculates the amount of funds it should keep in reserves and transfers any surplus to the `AccountingEngine`. Note that there is a `surplusTransferDelay` time delay between recalculating the optimum and transferring surplus out of the contract.



