---
description: Wrapper for FSM-like contracts
---

# FSM Wrapper

## 1. Summary <a id="1-introduction-summary"></a>

The `FSMWrapper` is meant to act as a funding source for FSM-like contracts as well as an interface that allows other contracts to read data from the FSM integrated with the wrapper.

## 2. Contract Variables & Functions <a id="2-contract-details"></a>

**Variables**

* `authorizedAccounts[usr: address]` - `addAuthorization`/`removeAuthorization` - auth mechanisms
* `lastReimburseTime` - last timestamp when the wrapper sent stability fee rewards to the address that called `fsm.updateResult()`
* `reimburseDelay` - enforced delay between consecutive `renumerateCaller` calls
* `fsm` - the FSM contract that's being wrapped; this contract is the only allowed caller for `renumerateCaller`

**Functions**

* `modifyParameters` - modify contract parameters
* `renumerateCaller(feeReceiver: address)` - called by the `fsm` in order to send stability fees from the [StabilityFeeTreasury](https://github.com/reflexer-labs/geb/blob/master/src/single/StabilityFeeTreasury.sol) to the `feeReceiver`
* `stopped() public view returns (uint256)` - read and return `stopped` from the `fsm`
* `priceSource() public view returns (address)` - read and return `priceSource` from the `fsm`
* `updateDelay() public view returns (uint16)` - read and return `updateDelay` from the `fsm`
* `lastUpdateTime() public view returns (uint64)` - read and return `lastUpdateTime` from the `fsm`
* `newPriceDeviation() public view returns (uint256)` - read and return the `newPriceDeviation` from the `fsm`
* `passedDelay() public view returns (bool)` - read and return `passedDelay` from the `fsm`
* `getNextBoundedPrice() public view returns (uint128)` - read and return the value calculated by `getNextBoundedPrice` from the `fsm`
* `getNextPriceLowerBound() public view returns (uint128)` - read and return the value calculated by `getNextPriceLowerBound` from the `fsm`
* `getNextPriceUpperBound() public view returns (uint128)` - read and return the value calculated by `getNextPriceUpperBound` from the `fsm`
* `getResultWithValidity() external view returns (uint256, bool)` - read and return the current result and its validity from the `fsm`
* `getNextResultWithValidity() external view returns (uint256, bool)` - read and return the next result and its validity from the `fsm`
* `read() external view returns (uint256)` - read and return \(or revert\) the current result from the `fsm` 

## 3. Walkthrough <a id="2-contract-details"></a>

Anyone can read values from the `fsm` contract by calling the wrapper `view` functions. The `fsm` contract is allowed to call `renumerateCaller` and thus send stability fee rewards to an address.

