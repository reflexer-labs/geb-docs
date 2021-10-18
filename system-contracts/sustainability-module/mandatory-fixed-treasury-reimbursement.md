---
description: Integration contract meant to offer a fixed reward pulled from the SF treasury
---

# Mandatory Fixed Treasury Reimbursement

## 1. Summary <a href="1-introduction-summary" id="1-introduction-summary"></a>

The `MandatoryFixedTreasuryReimbursement` is a contract meant to be inherited from and used as a way to offer a fixed stability fee reward (pulled from the SF treasury) to any address.

## 2. Contract Variables & Functions <a href="2-contract-details" id="2-contract-details"></a>

**Variables**

* `authorizedAccounts[usr: address]` - `addAuthorization`/`removeAuthorization` - auth mechanisms
* `fixedReward` - the fixed reward sent by the `treasury` to a fee receiver
* `treasury` - stability fee treasury contract

**Functions**

* `treasuryAllowance() public view returns (uint256)`** - **return the amount of SF that the treasury can transfer in one transaction when called by the reimbursement contract
* `getCallerReward() public view returns (uint256 reward)` - get the actual reward that can be pulled from the SF treasury by taking the minimum value between the `fixedReward`and the total amount that can be sent by the `treasury` in one block
* `rewardCaller(proposedFeeReceiver: address) internal` - internal function to send a SF reward to a fee receiver by calling the `treasury`

**Modifiers**

* `isAuthorized`** **- checks whether an address is part of `authorizedAddresses` (and thus can call authed functions)

**Events**

* `AddAuthorization` - emitted when a new address becomes authorized. Contains:
  * `account` - the new authorized account
* `RemoveAuthorization` - emitted when an address is de-authorized. Contains:
  * `account` - the address that was de-authorized
* `ModifyParameters` - emitted when a parameter is updated.
* `RewardCaller` - emitted when the contract rewards an address with SF coming from the `treasury`. Contains:
  * `finalFeeReceiver` - the address that got the reward
  * `fixedReward` - the reward that was sent

## 3. Walkthrough <a href="2-contract-details" id="2-contract-details"></a>

`rewardCaller` is the most important function in this contract. It takes care of pulling a fixed SF reward from the treasury and then sending it to a `proposedFeeReceiver`.

`getCallerReward` can be used to retrieve the current SF fee that can be pulled from the treasury.
