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



