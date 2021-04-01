---
description: >-
  Integration contract meant to offer an increasing reward pulled from the SF
  treasury
---

# Increasing Treasury Reimbursement

## 1. Summary <a id="1-introduction-summary"></a>

The `IncreasingTreasuryReimbursement` is a contract meant to be inherited from and used as a way to offer an increasing stability fee reward \(pulled from the SF treasury\) to any address.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `authorizedAccounts[usr: address]` - `addAuthorization`/`removeAuthorization` - auth mechanisms
* `baseUpdateCallerReward` - starting reward offer to a fee receiver
* `maxUpdateCallerReward` - max possible reward for a fee receiver
* `maxRewardIncreaseDelay` - max delay taken into consideration when calculating the adjusted reward
* `perSecondCallerRewardIncrease` - rate applied to `baseUpdateCallerReward` every extra second passed beyond a certain date \(e.g next time when a specific function needs to be called\)
* `treasury` - stability fee treasury contract

**Functions**

* `treasuryAllowance() public view returns (uint256)` - this returns the stability fee treasury allowance for the reimbursement contract by taking the minimum between the per block and the total allowances
* `getCallerReward(uint256 timeOfLastUpdate`, `uint256 defaultDelayBetweenCalls) public` `view returns (uint256)` - get the SF reward that can be sent to an address right now
* `rewardCaller(address proposedFeeReceiver, uint256 reward) internal` - internal function that's meant to send a SF reward to a `proposedFeeReceiver`

**Modifiers**

* `isAuthorized` ****- checks whether an address is part of `authorizedAddresses` \(and thus can call authed functions\)

**Events**

* `AddAuthorization` - emitted when a new address becomes authorized. Contains:
  * `account` - the new authorized account
* `RemoveAuthorization` - emitted when an address is de-authorized. Contains:
  * `account` - the address that was de-authorized
* `ModifyParameters` - emitted when a parameter is updated.
* `FailRewardCaller` - emitted when the contract cannot reward an address. Contains:
  * `revertReason` - the reason why the contract could not send the reward
  * `feeReceiver` - the address that was supposed to get the reward
  * `amount` - the reward that had to be sent

## 3. Walkthrough <a id="2-contract-details"></a>

`rewardCaller` is the most important function in this contract. It takes care of pulling the SF reward from the treasury and then sending it to a `proposedFeeReceiver`.

`getCallerReward` can be used to retrieve the current SF fee that can be pulled from the treasury. If `baseUpdateCallerReward` and `maxUpdateCallerReward` are both zero or if `timeOfLastUpdate >= now`, it will return zero.

{% hint style="warning" %}
**Invalid `maxRewardIncreaseDelay` set**

If `perSecondCallerRewardIncrease` is set to a large value and `maxRewardIncreaseDelay` is also large, `getCallerReward` may revert. In most scenarios,  `maxRewardIncreaseDelay` should be set to a very conservative value \(a couple of hours at most\) in order to avoid this scenario.
{% endhint %}



