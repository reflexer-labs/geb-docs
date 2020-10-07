---
description: DSPause cousin where transactions can be protested against and further delayed
---

# DSProtestPause

## 1. Summary <a id="1-introduction-summary"></a>

`DSProtestPause` inherits the same functionality from `DSPause` and it adds the possibility to further delay every transaction.

## 2. Contract Variables & Functions

**Variables**

* `scheduledTransactions[hashedTx: bytes32]` - mapping with all scheduled transactions. A 

  `hashedTx` consists of an address `usr` to `delegatecall` into, the expected codehash of `usr`, `calldata` to use and the first possible time of execution

* `transactionDelays[partiallyHashedTx: bytes32]` - data about each transaction's total delay and schedule time as well as whether it was protested against
* `proxy` - the proxy contract that will execute `scheduledTransactions` in an isolated environment. It can only be called by `DSProtestPause`
* `delay` - the delay applied to every `scheduledTransaction`
* `delayMultiplier` - the current delay multiplier applied to a transaction in case it is protested against
* `deploymentTime` - timestamp when the contract was deployed
* `protesterLifetime` - time \(post deployment\) during which the protester can call `protestAgainstTransaction`
* `protestEnd` - threshold after a transaction has been scheduled until that transaction can be protested against \(e.g the transaction delay is 2 hours, `protestEnd` is set to `500` and so the `protester` has 1 hour to protest against that transaction\)
* `currentlyScheduledTransactions` - the current number of concurrently scheduled transactions
* `maxScheduledTransactions` - the max number of transactions that can be scheduled at the same time
* `MAX_DELAY` - the maximum delay for a transaction from the moment it is scheduled
* `DS_PAUSE_TYPE` - the type of the `DSPause` contract \(can be `BASIC` or `PROTEST`\)

**Data Structures**

* `TransactionDelay` - struct containing data about each transaction's total delay and schedule time as well as whether it was protested against. Contains:
  * `protested` - whether a transaction has already been protested against
  * `scheduleTime` - timestamp when a transaction was scheduled
  * `totalDelay` - the total delay imposed on the transaction

**Modifiers**

* `isDelayed` - checks if `msg.sender` is `DSProtestPause` itself

**Functions**

* `getTransactionDataHash` - get the hash of a scheduled transaction
* `protestWindowAvailable(usr: address`, `codeHash: bytes32`, `parameters: bytes) external view returns (bool)` - checks whether a transaction can still be protested against
* `timeUntilProposalProtestDeadline(usr: address`, `codeHash: bytes32`, `parameters: bytes)` - returns the remaining time until a protested transaction can be executed 
* `setOwner(owner: address)` - set a new contract owner. Overriden from `DSAuth`
* `setAuthority(authority: DSAuthority)` - set an authority contract. Overriden from `DSAuth`
* `setDelay(delay: uint256)` - set a delay applied to all `scheduledTransactions`
* `scheduleTransaction(usr:address`, `codeHash: bytes32`, `parameters: bytes`, `earliestExecutionTime: uint)` - schedule a new transaction by providing `usr`, `usr`'s codehash, the calldata and the first possible time of execution
* `scheduleTransaction(usr: address`, `codeHash: bytes32`, `parameters: bytes`, `earliestExecutionTime: uint`, `description: string)` - schedule a new transaction and attach a description to it
* `attachTransactionDescription(usr: address`, `codeHash: bytes32`, `parameters: bytes`, `earliestExecutionTime: uint`, `description: string)` - attach a description for an already scheduled transaction
* `abandonTransaction(usr:address`, `codeHash: bytes32`, `parameters: bytes`, `earliestExecutionTime: uint)`- delete a scheduled transaction
* `protestAgainstTransaction(usr: address`, `codeHash: bytes32`, `parameters: bytes)` - protest against a transaction and delay it even more than it already is
* `executeTransaction(usr:address`, `codeHash: bytes32`, `parameters: bytes`, `earliestExecutionTime: uint)`- execute a scheduled transaction. Throws if the `scheduledTransaction`'s `delay` has not yet passed

**Events**

* `SetDelay` - emitted when the `delay` is changed. Contains:
  * `delay` - the new delay
* `ChangeDelayMultiplier` - emitted when the `delayMultiplier` is changed. Contains:
  * `multiplier` - the new `delayMultiplier`
* `ScheduleTransaction` - emitted when a new transaction is scheduled. Contains:
  * `sender` - the `msg.sender` that scheduled the transaction
  * `usr` - the target contract
  * `codeHash` - the code hash of `usr`
  * `parameters` - parameters for the transaction
  * `earliestExecutionTime` - earliest time when the transaction can be executed
* `AbandonTransaction` - emitted when governance abandons a previously scheduled transaction. Contains:
  * `sender` - the `msg.sender` that scheduled the transaction
  * `usr` - the target contract
  * `codeHash` - the code hash of `usr`
  * `parameters` - parameters for the transaction
  * `earliestExecutionTime` - earliest time when the transaction can be executed
* `ProtestAgainstTransaction` - emitted when a transaction is protested against. Contains:
  * `sender` - the `msg.sender` that scheduled the transaction
  * `usr` - the target contract
  * `codeHash` - the code hash of `usr`
  * `parameters` - parameters for the transaction
  * `totalDelay` - the new total delay for the transaction
* `ExecuteTransaction` - emitted when a transaction is executed. Contains:
  * `sender` - the `msg.sender` that scheduled the transaction
  * `usr` - the target contract
  * `codeHash` - the code hash of `usr`
  * `parameters` - parameters for the transaction
  * `earliestExecutionTime` - earliest time when the transaction can be executed
* `AttachTransactionDescription` - emitted when governance attaches a description to a scheduled transaction. Contains:
  * `sender` - the `msg.sender` that scheduled the transaction
  * `usr` - the target contract
  * `codeHash` - the code hash of `usr`
  * `parameters` - parameters for the transaction
  * `earliestExecutionTime` - earliest time when the transaction can be executed
  * `description` - the transaction description

## 3. Walkthrough <a id="2-contract-details"></a>

`DSProtestPause` is very similar to `DSPause` in that it is designed to be used as a component in a governance system and it gives affected parties time to respond to decisions. It also has a special extra function: it allows a `protester` to further delay the execution of any transaction \(e.g set the delay from 2 to 4 hours for a transaction\). 

This feature is especially useful in case the authorized contracts or EOAs allowed to `scheduleTransaction`s are captured by malicious actors.

