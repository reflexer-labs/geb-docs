---
description: Delegatecall based proxy with an enforced delay
---

# DSPause

## 1. Summary <a id="1-introduction-summary"></a>

`DSPause` allows authorized users to schedule function calls that can only be executed once some predetermined waiting period has elapsed. The configurable `delay` attribute sets the minimum wait time between scheduling and executing a transaction.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `scheduledTransactions[hashedTx: bytes32]` - mapping with all scheduled transactions. A

  `hashedTx` consists of an address `usr` to `delegatecall` into, the expected codehash of `usr`, `calldata` to use and the first possible time of execution

* `proxy` - the proxy contract that will execute `scheduledTransactions` in an isolated environment. It can only be called by `DSPause`
* `delay` - the delay applied to every `scheduledTransaction`
* `currentlyScheduledTransactions` - the current number of concurrently scheduled transactions
* `maxScheduledTransactions` - the max number of transactions that can be scheduled at the same time
* `MAX_DELAY` - the maximum delay for a transaction from the moment it is scheduled
* `DS_PAUSE_TYPE` - the type of the `DSPause` contract \(can be `BASIC` or `PROTEST`\)

**Modifiers**

* `isDelayed` - checks if `msg.sender` is `DSPause` itself

**Functions**

* `setOwner(owner: address)` - set a new contract owner. Overriden from `DSAuth`
* `setAuthority(authority: DSAuthority)` - set an authority contract. Overriden from `DSAuth`
* `setDelay(delay: uint256)` - set a delay applied to all `scheduledTransactions`
* `scheduleTransaction(usr:address`, `codeHash: bytes32`, `parameters: bytes`, `earliestExecutionTime: uint)` - schedule a new transaction by providing `usr`, `usr`'s codehash, the calldata and the first possible time of execution
* `scheduleTransaction(usr: address`, `codeHash: bytes32`, `parameters: bytes`, `earliestExecutionTime: uint`, `description: string)` - schedule a new transaction and attach a description to it
* `attachTransactionDescription(usr: address`, `codeHash: bytes32`, `parameters: bytes`, `earliestExecutionTime: uint`, `description: string)` - attach a description for an already scheduled transaction
* `abandonTransaction(usr:address`, `codeHash: bytes32`, `parameters: bytes`, `earliestExecutionTime: uint)`- delete a scheduled transaction
* `executeTransaction(usr:address`, `codeHash: bytes32`, `parameters: bytes`, `earliestExecutionTime: uint)`- execute a scheduled transaction. Throws if the `scheduledTransaction`'s `delay` has not yet passed

**Events**

* `SetDelay` - emitted when the `delay` is changed. Contains:
  * `delay` - the new delay
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

## 3. Walkthrough <a id="3-key-mechanisms-and-concepts"></a>

`DSPause` is designed to be used as a component in a governance system, to give affected parties time to respond to decisions. If those affected by governance decisions have e.g. exit or veto rights, then the pause can serve as an effective check on governance power.

### Scheduled Transactions

A `scheduledTransaction` describes a single `delegatecall` operation and a unix timestamp `earliestExecutionTime` before which it cannot be executed.

A `scheduledTransaction` consists of:

* `usr`: address to `delegatecall` into
* `codeHash`: the expected codehash of `usr`
* `parameters`: `calldata` to use
* `earliestExecutionTime`: first possible time of execution \(as seconds since unix epoch\)

Each scheduled tx has a unique id, defined as `keccack256(abi.encode(usr, codeHash, parameters, earliestExecutionTime))`

### Transaction Execution

In order to protect the internal storage of the pause from malicious writes during `scheduledTransaction` execution, we perform the actual `delegatecall` operation in a seperate contract with an isolated storage context \(`DSPauseProxy`\). Each pause has it's own individual `proxy`.

This means that `scheduledTransactions` are executed with the identity of the `proxy`, and when integrating the pause into some auth scheme, you probably want to trust the pause's `proxy` and not the pause itself.


## 4. Gotchas \(Potential source of user error\) <a id="4-gotchas"></a>

#### **Identity & Trust**

In order to protect the internal storage of the pause from malicious writes during proposal execution, we perform the `delegatecall` operation in a separate contract with an isolated storage context \(DSPauseProxy\), where each pause has its own individual proxy.

This means that proposals are executed with the identity of the `proxy`. Thus when integrating the pause into some auth scheme, you will want to trust the pause's proxy and not the pause itself.

## 5. Failure Modes \(Bounds on Operating Conditions & External Risk Factors\) <a id="5-failure-modes"></a>

**A break of any of the following would be classified as a critical issue:**

**High level**

* There is no way to bypass the delay
* The code executed by the delegatecall cannot directly modify storage on the pause
* The pause will always retain ownership of it's proxy

**Administrative**

* authority, owner, and delay can only be changed if an authorized user schedules a proposal to do so

**Schedule**

* A proposal can only be executed if its eta is after block.timestamp + delay
* A proposal can only be scheduled by authorized users

**Execute**

* A proposal can only be executed if it has previously been scheduled
* A proposal can only be executed once it's eta has passed
* A proposal can only be executed if its tag matches extcodehash\(usr\)
* A proposal can only be executed once
* A proposal can be executed by anyone

**Abandon**

* A proposal can only be abandoned by authorized users

#### Other Failure Modes

`DSPause.delay` - when the pause delay is set to the maximum, governance can no longer modify the system.

`DSPause.delay` - when the pause delay is set to the minimum, it is easier to pass malicious governance actions.

