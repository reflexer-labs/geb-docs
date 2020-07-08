---
description: Delegatecall based proxy with an enforced delay
---

# DSPause

## 1. Summary <a id="1-introduction-summary"></a>

`DSPause` allows authorized users to schedule function calls that can only be executed once some predetermined waiting period has elapsed. The configurable `delay` attribute sets the minimum wait time.

## 2. Contract Variables & Functions

* `setOwner(owner_: address)` - set a new contract owner. Overriden from `DSAuth`
* `setAuthority(DSAuthority authority_)` - set an authority contract. Overriden from `DSAuth`
* `setDelay(delay_: uint256)` - set a delay applied to all `scheduledTransactions`
* `scheduledTransactions` - mapping with all scheduled transactions. A transaction consists of an address `usr` to `delegatecall` into, the expected codehash of `usr`, `calldata` to use and the first possible time of execution \(as seconds since unix epoch\)
* `proxy` - the proxy contract that will execute `scheduledTransactions` in an isolated environment. It can only be called by `DSPause`
* `delay` - the delay applied to every `scheduledTransaction`
* `scheduleTransaction` - schedule a new transaction by providing `usr`, `usr`'s codehash, the calldata and the first possible time of execution
* `abandonTransaction` - delete a scheduled transaction
* `executeTransaction` - execute a scheduled transaction. Throws if the `scheduledTransaction`'s `delay` has not yet passed

## 3. Walkthrough <a id="2-contract-details"></a>

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



