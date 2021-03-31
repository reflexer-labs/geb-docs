---
description: Wrapper for FSM-like contracts
---

# FSM Wrapper

## 1. Summary <a id="1-introduction-summary"></a>

The `FSMWrapper` is meant to act as a funding source for FSM-like contract**s** as well as an interface that allows other contracts to read data from the FSM integrated with the wrapper.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `authorizedAccounts[usr: address]` - `addAuthorization`/`removeAuthorization` - auth mechanisms
* `lastReimburseTime` - last timestamp when the wrapper sent stability fee rewards to the address that called `fsm.updateResult()`
* `reimburseDelay` - enforced delay between consecutive `renumerateCaller` calls
* `fsm` - the FSM contract that's being wrapped; this contract is the only allowed caller for `renumerateCaller`
* `modifyParameters` - modify contract parameters
* `renumerateCaller(feeReceiver: address)` - 
* stopped\(\) public view returns \(uint256\) -
* priceSource\(\) public view returns \(address\) -
* updateDelay\(\) public view returns \(uint16\) -
* lastUpdateTime\(\) public view returns \(uint64\) -
* newPriceDeviation\(\) public view returns \(uint256\) -
* passedDelay\(\) public view returns \(bool\) -
* getNextBoundedPrice\(\) public view returns \(uint128\) -
* getNextPriceLowerBound\(\) public view returns \(uint128\) -
* getNextPriceUpperBound\(\) public view returns \(uint128\) -
* getResultWithValidity\(\) external view returns \(uint256, bool\) -
* getNextResultWithValidity\(\) external view returns \(uint256, bool\) -
* read\(\) external view returns \(uint256\) -



