# FLX Staking

## 1. Overview

Stakers are in charge with protecting the RAI protocol from insolvency. They stake [FLX/ETH Uniswap v2 LP tokens](https://v2.info.uniswap.org/pair/0xd6f3768e62ef92a9798e5a8cedd2b78907cecef9) in a smart contract that then constantly checks whether RAI is well capitalized.

In case the protocol is undercapitalized \(has debt that is not backed by collateral\), the staking pool will start to auction FLX/ETH LP tokens in exchange for RAI that is then used to bring the protocol above water. Eseentially, stakers will get diluted in this process.  
  
In exchange for protecting the protocol, stakers receive more FLX.

## 2. Staking Pool Parameters

### Mainnet

* Exit delay \(thawing period\): 21 days
* Percentage of rewards vested: 0% \(temporary, may change later\)
* Percentage of pool that can be slashed/auctioned: 30%

### Kovan

* Exit delay \(thawing period\): 1 minute
* Percentage of rewards vested: 0% \(temporary, may change later\)
* Percentage of pool that can be slashed/auctioned: 30%

## 3. Staking Walkthrough

First, you need to provide liquidity in this [FLX/ETH Uniswap v2 pool](https://app.uniswap.org/#/add/v2/0x6243d8cea23066d098a15582d81a598b4e8391f4/ETH) for mainnet and in [this pool](https://app.uniswap.org/#/add/v2/0x6e6eA84bb2fcE17AfCE8e1117DdC708142ef51c9/ETH) for Kovan.

![FLX/ETH Uniswap v2 Pool](../.gitbook/assets/lp.png)

Once you receive your FLX/ETH LP tokens, head over [here](https://app.reflexer.finance/earn/staking) \(for mainnet\) and [here](https://app-kovan.reflexer.finance/earn/staking) \(for Kovan\) in order to see the staking dashboard.

![](../.gitbook/assets/staking.png)

In the Stake section, you can specify how many LP tokens you'd like to stake. 

![](../.gitbook/assets/stake.png)

Once you stake, you will start to accrue rewards every block. You can claim rewards anytime using the **Claim Reward** button.

When you want to start unstaking, you can head over to the **Unstaking** section.

![](../.gitbook/assets/unstake.png)

Unstaking must be done in two stages:

* You request that a portion of your currently staked tokens should be unstaked
* You wait for the full thawing period until you can get your LP tokens back from the staking contract

![Thawing period passed and you can now withdraw your LP tokens](../.gitbook/assets/stflx.png)

**There are also several important things you must keep in mind when you unstake:**

* The amount of tokens that are waiting to be unstaked **do not** count toward accruing rewards anymore. You only accrue rewards for the tokens that are both staked and are not waiting to be unstaked
* Take the following scenario: Alice has 10 tokens staked. She requests to unstake 5 tokens and has to wait 4 weeks to withdraw them from the contract. After 3 weeks, Alice requests that she unstakes another 3 extra tokens \(so she's only earning rewards with 2 tokens now\). Alice must wait 4 weeks from the moment of the **second unstake request** in order to withdraw the whole 8 tokens from the contract
* Take another scenario: Alice has 10 tokens staked. She requests to unstake 5 tokens and has to wait 4 weeks to withdraw them from the contract. After 5 weeks, Alice does not unstake the 5 tokens but rather requests that she unstakes another 3 extra tokens \(so she's only earning rewards with 2 tokens now\). Alice must wait 4 weeks from the moment of the **second unstake request** even if the 5 tokens could have been withdrawn from the contract

Anothing thing to keep in mind: after you stake, the staking pool will mint a claim token for you called **Staked FLX \(stFLX\)**. **stFLX is not transferrable.**

You will notice that the amount of stFLX you receive might differ from the amount of LP tokens you stake. That is normal behaviour.
