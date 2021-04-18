---
description: Building and integrating insurance contracts for SAFEs
---

# SAFE Insurance

## 1. Overview

The GEB [LiquidationEngine](https://github.com/reflexer-labs/geb/blob/master/src/LiquidationEngine.sol) allows governance to [whitelist](https://github.com/reflexer-labs/geb/blob/a49e4486682b787571475821ec66bfa025e5183f/src/LiquidationEngine.sol#L88) external insurance contracts for `SAFE`s. `SAFE` users can [attach](https://github.com/reflexer-labs/geb/blob/a49e4486682b787571475821ec66bfa025e5183f/src/LiquidationEngine.sol#L290) insurance contracts to their positions and this way have an extra layer of protection against liquidation.

Anyone can build and propose new insurance contracts, assuming that the contracts abide by the requirements and principles outlined below. A central repository with `SAFE` insurance contracts \(also called saviours\) and interfaces can be found [here](https://github.com/reflexer-labs/geb-safe-saviours).

{% hint style="info" %}
**Important Note**

The sections below only cover saviours that add collateral in a SAFE and don't pay back a SAFE's debt. 

It's worth noting that saviours may only pay back debt instead of adding any collateral.
{% endhint %}

## 2. Contract Interface

Every insurance contract must implement one of the official interfaces \(the oldest interface can be found [here](https://github.com/reflexer-labs/geb-safe-saviours/blob/master/src/interfaces/SafeSaviourLike.sol)\):

```javascript
abstract contract SafeSaviourLike is ReentrancyGuard {
    // Checks whether a saviour contract has been approved by governance in the LiquidationEngine
    modifier liquidationEngineApproved(address saviour) {
        require(liquidationEngine.safeSaviours(saviour) == 1, "SafeSaviour/not-approved-in-liquidation-engine");
        _;
    }
    // Checks whether someone controls a safe handler inside the GebSafeManager
    modifier controlsSAFE(address owner, uint256 safeID) {
        require(owner != address(0), "SafeSaviour/null-owner");
        require(either(owner == safeManager.ownsSAFE(safeID), safeManager.safeCan(safeManager.ownsSAFE(safeID), safeID, owner) == 1), "SafeSaviour/not-owning-safe");

        _;
    }

    // --- Variables ---
    LiquidationEngineLike   public liquidationEngine;
    OracleRelayerLike       public oracleRelayer;
    GebSafeManagerLike      public safeManager;
    SAFEEngineLike          public safeEngine;
    SAFESaviourRegistryLike public saviourRegistry;

    // The amount of tokens the keeper gets in exchange for the gas spent to save a SAFE
    uint256 public keeperPayout;          // [wad]
    // The minimum fiat value that the keeper must get in exchange for saving a SAFE
    uint256 public minKeeperPayoutValue;  // [wad]
    /*
      The proportion between the keeperPayout (if it's in collateral) and the amount of collateral or debt that's in a SAFE to be saved.
      Alternatively, it can be the proportion between the fiat value of keeperPayout and the fiat value of the profit that a keeper
      could make if a SAFE is liquidated right now. It ensures there's no incentive to intentionally put a SAFE underwater and then
      save it just to make a profit that's greater than the one from participating in collateral auctions
    */
    uint256 public payoutToSAFESize;

    // --- Constants ---
    uint256 public constant ONE               = 1;
    uint256 public constant HUNDRED           = 100;
    uint256 public constant THOUSAND          = 1000;
    uint256 public constant WAD_COMPLEMENT    = 10**9;
    uint256 public constant WAD               = 10**18;
    uint256 public constant RAY               = 10**27;
    uint256 public constant MAX_UINT          = uint(-1);

    // --- Boolean Logic ---
    function both(bool x, bool y) internal pure returns (bool z) {
        assembly{ z := and(x, y) }
    }
    function either(bool x, bool y) internal pure returns (bool z) {
        assembly{ z := or(x, y)}
    }

    // --- Events ---
    event SaveSAFE(address indexed keeper, bytes32 indexed collateralType, address indexed safeHandler, uint256 collateralAddedOrDebtRepaid);

    // --- Functions to Implement ---
    function saveSAFE(address,bytes32,address) virtual external returns (bool,uint256,uint256);
    function getKeeperPayoutValue() virtual public returns (uint256);
    function keeperPayoutExceedsMinValue() virtual public returns (bool);
    function canSave(bytes32,address) virtual external returns (bool);
    function tokenAmountUsedToSave(bytes32,address) virtual public returns (uint256);
}

```

## 3. Implementation Guidelines

In order to get an idea of how a saviour contract should be implemented and what checks must be in place, let's analyze the components of a [demo contract](https://github.com/reflexer-labs/geb-safe-saviours/blob/master/src/saviours/GeneralTokenReserveSafeSaviour.sol) that allows `SAFE` users to deposit & withdraw collateral used to save their positions.

### Constructor Requirements:

* Sanitize every parameter \(`address`es must be non null, `uint` values are non null and within expected bounds etc\)
* In case of a saviour that adds more collateral in Safes, you must set the [CollateralJoin](https://github.com/reflexer-labs/geb-deploy/blob/master/src/AdvancedTokenAdapters.sol) contract of the specific collateral type you're targeting
* In case of a saviour that repays debt instead of adding collateral, you must set the [CoinJoin](https://github.com/reflexer-labs/geb/blob/dc6ad8302080cb2912f8d5fb258cdcae13601982/src/BasicTokenAdapters.sol#L225) contract
* Every saviour type should also have the [LiquidationEngine](https://github.com/reflexer-labs/geb/blob/master/src/LiquidationEngine.sol), [OracleRelayer](https://github.com/reflexer-labs/geb/blob/master/src/OracleRelayer.sol), [SAFEEngine](https://github.com/reflexer-labs/geb/blob/master/src/SAFEEngine.sol), [GebSafeManager](https://github.com/reflexer-labs/geb-safe-manager/blob/master/src/GebSafeManager.sol), [SAFESaviourRegistry](https://github.com/reflexer-labs/geb-safe-saviours/blob/master/src/SAFESaviourRegistry.sol) and [SaviourCRatioSetter](https://github.com/reflexer-labs/geb-safe-saviours/blob/master/src/SaviourCRatioSetter.sol) set
* You must set: 
  * `keeperPayout` - amount of collateral awarded to the address that initially called [LiquidationEngine.liquidateSAFE](https://github.com/reflexer-labs/geb/blob/a49e4486682b787571475821ec66bfa025e5183f/src/LiquidationEngine.sol#L309)
  * `minKeeperPayoutValue` - the minimum fiat value of the `keeperPayout` which makes it compelling for keepers to save the `SAFE` instead of waiting even more to liquidate it
  * `defaultDesiredCollateralizationRatio` - the default CRatio that a `SAFE` will have after it's saved; this CRatio must be greater than the liquidation ratio stored in the [OracleRelayer](https://github.com/reflexer-labs/geb/blob/a49e4486682b787571475821ec66bfa025e5183f/src/OracleRelayer.sol#L60) and that is associated with `collateralToken`
* Optionally, you can set:
  * `payoutToSAFESize` - how many times more collateral there must be in a `SAFE` compared to `keeperPayout`; this prevents keepers from purposefully liquidating `SAFE`s so they get a reward that is bigger than the one offered in a collateral auction
* When comparing a `liquidationCRatio` from the `OracleRelayer` with a desired collateralization ratio, you must first divide `liquidationCRatio` by `CRATIO_SCALE_DOWN` so you have the same scale for both numbers
* You must integrate your saviour with [GebSafeManager](https://github.com/reflexer-labs/geb-safe-manager/blob/master/src/GebSafeManager.sol) in order to take advantage of its modularity and friendlier interface compared to core contracts \(such as `SAFEEngine`\)

### Covering & Uncovering SAFEs:

There is no specific way in which users should cover a `SAFE`. They can store collateral in the saviour, they can also store [aTokens](https://docs.aave.com/developers/the-core-protocol/atokens) or [cTokens](https://compound.finance/docs/ctokens) that are then used to redeem the underlying assets and add them in the SAFE or they can use a protocol similar to [Nexus Mutual](https://nexusmutual.gitbook.io/docs/users/docs) which automatically fulfils claims and saves positions. There are, though, certain things that a saviour developer must take into account:

* The function used to add more cover for a `SAFE` \(like [deposit](https://github.com/reflexer-labs/geb-safe-saviours/blob/6b8a89e1f6e7c7d210cb68f684ac1c6a5fb6e0c5/src/saviours/GeneralTokenReserveSafeSaviour.sol#L85)\) must revert if the saviour contract is not whitelisted inside the [LiquidationEngine](https://github.com/reflexer-labs/geb/blob/a49e4486682b787571475821ec66bfa025e5183f/src/LiquidationEngine.sol#L83)
* Users can only add cover if their SAFEs [have generated debt](https://github.com/reflexer-labs/geb-safe-saviours/blob/6b8a89e1f6e7c7d210cb68f684ac1c6a5fb6e0c5/src/saviours/GeneralTokenReserveSafeSaviour.sol#L95)
* Users can withdraw cover \(with something like [withdraw](https://github.com/reflexer-labs/geb-safe-saviours/blob/6b8a89e1f6e7c7d210cb68f684ac1c6a5fb6e0c5/src/saviours/GeneralTokenReserveSafeSaviour.sol#L109)\) even if the saviour contract is not whitelisted inside the [LiquidationEngine](https://github.com/reflexer-labs/geb/blob/a49e4486682b787571475821ec66bfa025e5183f/src/LiquidationEngine.sol#L83)
* Only the `SAFE`'s owner or an authorized address inside the [GebSafeManager](https://github.com/reflexer-labs/geb-safe-manager/blob/7da11a4638a994fb0a58e7c1330f69ce897844f9/src/GebSafeManager.sol#L49) can withdraw cover

{% hint style="danger" %}
**Reentrancy**

Make sure to protect your cover/uncover functions against re-entrancy.
{% endhint %}

### View Function Requirements:

[keeperPayoutExceedsMinValue](https://github.com/reflexer-labs/geb-safe-saviours/blob/6b8a89e1f6e7c7d210cb68f684ac1c6a5fb6e0c5/src/saviours/GeneralTokenReserveSafeSaviour.sol#L214):

* It must verify if the fiat value of `keeperPayout` exceeds or is equal to `minKeeperPayoutValue`
* It must read the collateral's price from the [same oracle](https://github.com/reflexer-labs/geb-safe-saviours/blob/6b8a89e1f6e7c7d210cb68f684ac1c6a5fb6e0c5/src/saviours/GeneralTokenReserveSafeSaviour.sol#L215) used inside `OracleRelayer` in order to maintain consistency between the core system and the saviour
* It must return `false` if the oracle price is invalid

[getKeeperPayoutValue](https://github.com/reflexer-labs/geb-safe-saviours/blob/6b8a89e1f6e7c7d210cb68f684ac1c6a5fb6e0c5/src/saviours/GeneralTokenReserveSafeSaviour.sol#L227):

* It must read the collateral's price from the [same oracle](https://github.com/reflexer-labs/geb-safe-saviours/blob/6b8a89e1f6e7c7d210cb68f684ac1c6a5fb6e0c5/src/saviours/GeneralTokenReserveSafeSaviour.sol#L228) used inside `OracleRelayer` in order to maintain consistency between the core system and the saviour
* It must return `0` if the oracle price is invalid
* It must return the fiat value of `keeperPayout` collateral tokens used to pay keepers for saving `SAFE`s

[tokenAmountUsedToSave](https://github.com/reflexer-labs/geb-safe-saviours/blob/6b8a89e1f6e7c7d210cb68f684ac1c6a5fb6e0c5/src/saviours/GeneralTokenReserveSafeSaviour.sol#L256):

* It must return the amount of collateral tokens that will be used to save a `SAFE` and bring its CRatio to the desired ratio
* It must read the collateral's price from the [same oracle](https://github.com/reflexer-labs/geb-safe-saviours/blob/6b8a89e1f6e7c7d210cb68f684ac1c6a5fb6e0c5/src/saviours/GeneralTokenReserveSafeSaviour.sol#L259) used inside `OracleRelayer` in order to maintain consistency between the core system and the saviour
* It must return early if the targeted `SAFE` has no debt or if the oracle feed is invalid

[canSave](https://github.com/reflexer-labs/geb-safe-saviours/blob/6b8a89e1f6e7c7d210cb68f684ac1c6a5fb6e0c5/src/saviours/GeneralTokenReserveSafeSaviour.sol#L242):

* It must return `true` if a `SAFE` can currently be saved, `false` if not
* It must check that, when the `SAFE` is saved, the contract has enough tokens to both reward the keeper that called `LiquidationEngine.liquidateSAFE` and also bring the `SAFE`'s CRatio to the desired level

### Saving a SAFE:

The process of saving a `SAFE` has its own requirements:

* You must implement and use `saveSAFE(address keeper`, `bytes32 collateralType`, `address safeHandler) external returns (bool`, `uint256`, `uint256)` in order to save `SAFE`s
* `saveSAFE` must check that `msg.sender` is the `LiquidationEngine`
* The `keeper` parameter must not be null
* There is a special condition you must add where, if the `collateralType` is null, the `keeper` is the `LiquidationEngine` itself and the `safeHandler` is null, you return a tuple like `(true`, `uint(-1)`, `uint(-1))`. This condition will help the `LiquidationEngine` to check that you implemented `saveSAFE`. You can see an example of this condition [here](https://github.com/reflexer-labs/geb-safe-saviours/blob/6b8a89e1f6e7c7d210cb68f684ac1c6a5fb6e0c5/src/saviours/GeneralTokenReserveSafeSaviour.sol#L157)
* You must check that `keeperPayoutExceedsMinValue` returns `true`. If it returns `false`, you must return early
* You must check that the `SAFE` has debt in it
* You must check that the saviour can both reward the keeper for saving the SAFE and also add enough collateral in the SAFE so its CRatio goes to the desired level
* You must **not** add any collateral in the `SAFE` or repay the `SAFE`'s debt in case it **cannot** be saved \(its CRatio cannot be increased to the desired level\). You must revert in case the `SAFE` cannot be saved
* You must call `saviourRegistry.markSave(collateralType`, `safeHandler)` so that the `SAFESaviourRegistry` knows a specific `SAFE` has just been saved. The registry enforces a delay between two consecutive save actions for a specific `SAFE`. The delay is there to make sure that `SAFE` users don't solely rely on saviours to protect their positions. This way we avoid a scenario where one or a couple of popular saviours fail \(e.g bugs, lack of sufficient cover\) and most positions in the system are liquidated at once
* You must emit a `SaveSAFE` event before you return
* The last thing you have to do is to return a tuple in the form of `(true`, `tokenAmountUsed`, `keeperPayout)` where:
  * `tokenAmountUsed` is the amount of collateral that was used to save the `SAFE`
  * `keeperPayout` is a non null amount of collateral that was used to reward the keeper 

You can check an example implementation for `saveSAFE` [here](https://github.com/reflexer-labs/geb-safe-saviours/blob/6b8a89e1f6e7c7d210cb68f684ac1c6a5fb6e0c5/src/saviours/GeneralTokenReserveSafeSaviour.sol#L153).

## 4. Monetizing a Saviour

In some cases, saviour builders may want to monetize the service they provide and charge some sort of fee for protecting `SAFE`s.

If you would like to submit a proposal for implementing a monetized saviour, you must keep in mind several things:

* Monetization should happen outside of the saviour contract. This keeps concerns separated, simplifies the saviour implementation and gives you more flexibility when it comes to updating your business model
* You must mention that your saviour will be monetized and you should give a detailed description of how you plan to charge `SAFE` users. You must include the description in your GIP as outlined in the section below
* Assuming you plan to use a smart contract to charge users, your should attach a detailed overview or implementation of your model inside your GIP

## 5. Launching on Mainnet

In order to launch and integrate a saviour with a mainnet deployed GEB, the saviour must first pass several checks:

1. You must first create a new [GEB Improvement Proposal](https://github.com/reflexer-labs/GIPs). Once you create the GIP you must ask for feedback on [Reflexer's Discord server](https://discord.gg/kB4vcYs) \(in the development channel\). To maximize your chances of having your idea accepted:
   * Your saviour must only do one thing. For example, you should only handle aTokens or cTokens, not both
   * Your saviour should only take into account a single collateral type \(e.g ETH-A **or** ETH-B, **not** ETH-A and ETH-B\) in case it's meant to add collateral. If the saviour repays debt, it can be generalized to handle any Safe with any collateral type
   * You should have a draft implementation of your saviour with estimated gas amounts for calling each function
   * You should give an initial estimate of the `keeperPayout`, `minKeeperPayoutValue` and `payoutToSAFESize` values you plan to set
   * You must specify if you plan to monetize the saviour service you're building and how you plan to do it
2. Once you receive feedback \(and assuming it's positive\), you can start to fully implement the saviour. Keep in mind that, aside from the saviour, you will need to create a proxy actions contract \(like [this one](https://github.com/reflexer-labs/geb-proxy-actions/blob/f7b4cdb0cde25683dc7bb04cf96f25ebe25a1853/src/GebProxyActions.sol#L746)\) that will be used by others to connect your saviour to Safes and also add cover.
3. Before you submit your full implementation and update the GIP, you must make sure that you have 100% test coverage for your code and also do several integration tests between the `LiquidationEngine`, `SAFESaviourRegistry` and the saviour code. In order to submit your implementation, update your GIP with a link to your code and a new summary of the gas amounts required to call each function, as well as updated values for `keeperPayout`, `minKeeperPayoutValue`, `payoutToSAFESize` and `defaultDesiredCollateralizationRatio`

   . After you update the GIP, ping the community on Discord.

4. Once your implementation is accepted and reviewed by the community, your code must also be audited twice. Each audit must be done by an independent party.
5. After your code gets audited, you should send a new message on Discord and let the community know that it's ready to be integrated in production. You must link to the audit reports.
6. Governance may decide to first try out your saviour on a testnet. In this case, you must deploy an instance of your saviour on a testnet GEB and liquidate a `SAFE` which can then be saved
7. Assuming that you pass all previous steps, you can deploy your saviour on mainnet so that governance can whitelist it in `LiquidationEngine` and in `SAFESaviourRegistry`

## 6. Integration Ideas

To help you get started, here are a couple of ideas for building RAI saviours:

* Allow users to deposit [Aave aETH](https://aave.com/aTokens/) in a saviour which can then be used to redeem ETH that is then added in a `SAFE`
* Allow users to deposit [Compound cETH](https://compound.finance/docs/ctokens) in a saviour which can then be used to redeem ETH 

  that is then added in a `SAFE`

* [Opyn](https://opyn.co/#/) or [Hegic](https://www.hegic.co/) options which can be exercised when `saveSAFE` is called

