# GEB Risks

Using a GEB deployment and/or its associated stable asset doesn't come without risk. Before you decide to deposit your assets in the protocol or acquire the stable asset, you should do your research and understand the risks involved.

This section will only give an overview of the main risks associated with GEB. If you'd like to dive deeper, you can check out every [module](https://docs.reflexer.finance/system-contracts/core) in the System Contracts section.

You can also check our original [whitepaper](https://github.com/reflexer-labs/whitepapers).

### Smart Contract Bugs

The core GEB contracts were audited by [OpenZeppelin](https://github.com/reflexer-labs/geb-audits/tree/master/open-zeppelin/core-contracts). Other helper contracts were audited by [Quantstamp](https://github.com/reflexer-labs/geb-audits/tree/master/quantstamp/helper-contracts).

However, security audits do not completely eliminate smart contract risk. We urge you not to put your life savings or money you can't afford to lose into any GEB deployment or its associated stable asset.

### Admin Keys

The very first GEB deployment will need to be fully managed in its initial stages because of the risks tied to the PID controller managing the system as well as the need for more infrastructure to be built so the protocol can be automated.

Subsequent GEB deployments may or may not be governed, depending on whether the community will want to add more collateral types as time goes by.

While a GEB is fully managed/governed, almost all of its components can be upgraded and manually set up. Once it's governance minimized, only a few components can be upgraded and fewer parameters can be changed.

You can take a look at the [Governance Minimization Guide](https://docs.reflexer.finance/governance-minimization-guide) to see what will need to be done so that a GEB can be governance minimized. Stay alert for more updates from the Reflexer team regarding a timeline for RAI governance minimization.

### PID Controller

PID control is still a novel concept in DeFi. No other stable asset prior to RAI has been managed by an on-chain controller and there is no historical data that can help with the controller's modelling and simulations.

If the controller is too slow it may be completely ineffective in stabilizing RAI or other reflex indexes. If it's too strong, it may destabilize the system.

**NOTE**: check out more PID failure modes in [this section](https://docs.reflexer.finance/pid-failure-modes-and-responses).

### Suboptimal Parameters

Governance may set suboptimal parameters for:

* Debt auctions which can lead to an excessive amount of protocol tokens being printed
* Collateral auctions which may not give a good enough incentive for bidding
* Global Settlement which may delay SAFE processing and collateral redemption indefinitely

There are many more parameters which may be suboptimal. Check the [System Contracts modules](https://docs.reflexer.finance/system-contracts/core) for more details.

