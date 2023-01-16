---
description: Coin Savings Account
---

# Coin Savings Account - Detailed Documentation

## 1. Introduction (Summary)

Coin Savings Account allows users to deposit `coin` and activate the Coin Savings Rate and earning savings on their `coin`. The savings rate is set by Reflexer Governance, and will typically be less than the base stability fee to remain sustainable. The purpose of Coin Savings Account is to offer another incentive for holding Coin.

## 2. Contract Details

#### Math

* `mul(uint, uint)`, `rmul(uint, uint)`, `add(uint, uint)`& `sub(uint, uint)` - will revert on overflow or underflow
* `rpow(uint x, uint n, uint base)`, used for exponentiation in `updateAccumulatedRate`, is a fixed-point arithmetic function that raises `x` to the power `n`. It is implemented in assembly as a repeated squaring algorithm. `x` (and the result) are to be interpreted as fixed-point integers with scaling factor `base`. For example, if `base == 100`, this specifies two decimal digits of precision and the normal decimal value 2.1 would be represented as 210; `rpow(210, 2, 100)` should return 441 (the two-decimal digit fixed-point representation of 2.1^2 = 4.41). In the current implementation, 10^27 is passed for `base`, making `x` and the `rpow` result both of type `ray` in standard MCD fixed-point terminology. `rpow`’s formal invariants include “no overflow” as well as constraints on gas usage.

#### Auth

* `authorizedAccounts` are allowed to call protected functions (Administration)

#### Storage

* `savings` - stores the address' `Coin Savings Account` balance.
* `totalSavings` - stores the total balance in the `Coin Savings Account`.
* `savingsRate` - the `coin savings rate`. It starts as `1` (`ONE = 10^27`), but can be updated by governance.
* `accumulatedRates` - the rate accumulator. This is the always increasing value which decides how much `coin` - given when `updateAccumulatedRate()` is called.
* `safeEngine` - an address that conforms to a `SafeEngineLike` interface. It is set during the constructor and **cannot be changed**.
* `accountingEngine` - an address that conforms to a `accountingEngineLike` interface. Not set in constructor. Must be set by governance.
* `updateRime` - the last time that updateAccumulatedRate is called.

The values of `savingsRate` and `accountingEngine` can be changed by an authorized address in the contract (i.e. Reflexer Governance). The values of `accumulatedRates`, `savings`, `totalSavings`, and `updateTime` are updated internally in the contract and cannot be changed manually.

## 3. Key Mechanisms & Concepts

#### updateAccumulatedRate()

* Calculates the most recent `accumulatedRates` and pulls `coin` from the `safeEngine` (by increasing the `AccountingEngine`'s `debtBalance`).
* A user should always make sure that this has been called before calling the `withdraw()` function.
* updateAccumulatedRate has to be called before a user `deposit`s and it is in their interest to call it again before they `withdraw`, but there isn't a set rule for how often updateAccumulatedRate is called.

#### deposit(uint wad)

* `uint wad` this parameter is based on the amount of coin (since `wad` = `coin`/ `accumulatedRates` ) that you want to `deposit` to the `savingsAccount`. The `wad * accumulatedRates` must be present in the `safeEngine` and owned by the `msg.sender`.
* the `msg.sender`'s `savings` amount is updated to include the `wad`.
* the total `totalSavings` amount is also updated to include the `wad`.

#### withdraw(uint wad)

* `withdraw()` essentially functions as the exact opposite of `deposit()`.
* `uint wad` this parameter is based on the amount of coin that you want to `withdraw` the `savingsAccount`. The `wad * accumulatedRates` must be present in the `safeEngine` and owned by the `savingsAccount` and must be less than `msg.sender`'s `savings` balance.
* The `msg.senders` `savings` amount is updated by subtracting the `wad`.
* The total `totalSavings` amount is also updated by subtracting the `wad`.

#### Administration

Various modifyParameters function signatures for administering `Coin Savings Account`:

* Setting new savingsRate (`modifyParameters(bytes32, uint256)`)
* Setting new accountingEngine (`modifyParameters(bytes32, address)`)

### Usage

The primary usage will be for `addresses` to store their `coin` in the `savingsAccount` to accumulate interest over time

## 4. Gotchas / Integration Concerns

* The `savingsRate` is set (globally) through the governance system. It can be set to any number > 0%. This includes the possibility of it being set to a number that would cause the savings rate to accumulate faster than the collective Stability Fees, thereby accruing system debt and eventually causing FLX to be minted.
* If `updateAccumulatedRate()` has not been called recently before an address calls `withdraw()` they will not get the full amount they have earned over the time of their deposit.
* If a user wants to `deposit` or `withdraw` 1 DAI into/from the Coin Savings Account, they should send a `wad` = to `1 / accumulatedRates` as the amount moved from their balance will be `1 * accumulatedRates`

## 5. Failure Modes and Impact


#### Coding Error

A bug in the `Coin Savings Account` could lead to locking of `coin` if the `withdraw()` function or the underlying `safeEngine.createUnbackedDebt()` or `safeEngine.transferInternalCoins()` functions were to have bugs.

#### Governance

The `savingsRate` rate initially can be set through Governance. Governance will be able to change the savings rate based on the rules that the Governor employs (which would include a Pause for actions).

One serious risk is if governance chooses to set the `savingsRate` to an extremely high rate, this could cause the system's fees to be far too high. Furthermore, if governance allows the `savingsRate` to (significantly) exceed the system fees, it would cause debt to accrue and increase the Flop auctions.

