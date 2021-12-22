# FLX Mechanics

## 1. Overview

The FLX token has two main functions inside the RAI protocol:

* Backstop mechanism: FLX stakers are the first line of defense in case the RAI protocol goes underwater. The second line of defense is with debt auctions that mint new FLX and auction it in exchange for RAI
* Ungovernance: once governance minimization is finalized, FLX holders will be able to remove control from any remaining components in RAI or, if needed, continue to manage components that may be challenging to ungovern (such as oracles or any other component interacting with other protocols)

## 2. RAI Resource Flow

Before the protocol is governance minimized, RAI will be set up so that stability fees (borrow rate charged to Safes that mint RAI) flow in three places:

* The stability fee treasury, which is a smart contract in charge with paying for oracle updates or any other contract meant to automate RAI parameters
* FLX stakers, which are the first line of defense for the protocol
* Buyback and burn, which is meant to auction RAI in exchange for FLX which is subsequently burned

In the case of FLX stakers, the RAI that accrues for them is auctioned in exchange for FLX. The FLX proceeds from the auction are then sent to the staking pool.\
\
As for buyback and burn, RAI is first accrued in the protocol's balance sheet. Once there's enough RAI in the balance sheet, the protocol can start to auction some of it in exchange for FLX that is then burned.

To visualize all this, you can check the diagram below:

![](.gitbook/assets/untitled-diagram.png)

**NOTE**: RAI only flows to the stability fee treasury, to stakers and in the protocol's balance sheet when the borrow rate charged to Safes is positive. When the borrow rate is negative, the protocol only uses funds from the balance sheet to repay RAI debt from all Safes.

## 3. FLX and Money God League

FLX is also at the core of the Money God League as seen [here](https://community.reflexer.finance/t/another-new-money-god-introducing-h2o/131) and [here](https://community.reflexer.finance/t/a-new-money-god-introducing-volt/120). The Money God League is presented on this page.
