---
description: Building and integrating insurance contracts for SAFEs
---

# SAFE Insurance

## 1. Overview

The GEB [LiquidationEngine](https://github.com/reflexer-labs/geb/blob/master/src/LiquidationEngine.sol) allows governance to [whitelist](https://github.com/reflexer-labs/geb/blob/a49e4486682b787571475821ec66bfa025e5183f/src/LiquidationEngine.sol#L88) external insurance contracts for `SAFE`s. `SAFE` users can [attach](https://github.com/reflexer-labs/geb/blob/a49e4486682b787571475821ec66bfa025e5183f/src/LiquidationEngine.sol#L290) insurance contracts to their positions and this way have an extra layer of protection against liquidation.

Anyone can build and propose new insurance contracts, assuming that the contracts abide by the requirements and principles outlined below. A central repository with `SAFE` insurance contracts \(also called saviours\) and interfaces can be found [here](https://github.com/reflexer-labs/geb-safe-saviours).

### 2. Contract Interface

Every insurance contract must implement one of the official interfaces \(the oldest one can be found [here](https://github.com/reflexer-labs/geb-safe-saviours/blob/master/src/interfaces/SafeSaviourLike.sol)\):

```javascript
abstract contract SafeSaviourLike {
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
    // The contract used to liquidate SAFEs inside GEB
    LiquidationEngineLike   public liquidationEngine;
    // The contract that glues together collateral oracles and the SAFEEngine
    OracleRelayerLike       public oracleRelayer;
    /* 
       The GebSafeManager that the saviour is integrated with.
       The manager makes it easier for developers to integrate with the system
    */
    GebSafeManagerLike      public safeManager;
    // The contract maintaining the state of all SAFEs
    SAFEEngineLike          public safeEngine;
    // A registry that keeps that of when a SAFE has last been saved
    SAFESaviourRegistryLike public saviourRegistry;

    // The amount of tokens the keeper gets in exchange for the gas spent to save a SAFE
    uint256 public keeperPayout;
    // The minimum fiat value that the keeper must get in exchange for saving a SAFE
    uint256 public minKeeperPayoutValue;
    /*
      The proportion between the keeperPayout and the amount of collateral that's in the SAFE to be saved. It ensures there's no
      incentive to put a SAFE underwater and then save it just to make a profit
    */
    uint256 public payoutToSAFESize;
    // The default collateralization ratio a SAFE should have after it's saved
    uint256 public defaultDesiredCollateralizationRatio;

    // Desired CRatios for each SAFE after they're saved
    mapping(bytes32 => mapping(address => uint256)) public desiredCollateralizationRatios;

    // --- Constants ---
    uint256 public constant ONE               = 1;
    uint256 public constant HUNDRED           = 100;
    uint256 public constant THOUSAND          = 1000;
    uint256 public constant CRATIO_SCALE_DOWN = 10**25;
    uint256 public constant WAD               = 10**18;
    uint256 public constant RAY               = 10**27;
    uint256 public constant MAX_CRATIO        = 1000;
    uint256 public constant MAX_UINT          = uint(-1);

    // --- Boolean Logic ---
    function both(bool x, bool y) internal pure returns (bool z) {
        assembly{ z := and(x, y) }
    }
    function either(bool x, bool y) internal pure returns (bool z) {
        assembly{ z := or(x, y)}
    }

    // --- Functions to Implement ---
    // Mandatory function called by the LiquidationEngine in order to save a SAFE
    function saveSAFE(address,bytes32,address) virtual external returns (bool,uint256,uint256);
    // Returns the fiat value of the keeper's payout
    function getKeeperPayoutValue() virtual public returns (uint256);
    // Validates if the fiat value of the keeper's payout exceeds a min threshold
    function keeperPayoutExceedsMinValue() virtual public returns (bool);
    // Validates if a SAFE can be currently saved
    function canSave(address) virtual external returns (bool);
    /*
       Returns the amount of collateral tokens that will be used to save a
       SAFE so it has a desiredCollateralizationRatios CRatio afterwards
    */
    function tokenAmountUsedToSave(address) virtual public returns (uint256);
}
```

### 3. Implementation Guidelines

In order to get an idea of how a saviour contract should be implemented and what checks must be in place, let's analyze the components of a demo contract that allows `SAFE` users to deposit & withdraw collateral used to save their positions.

#### Constructor Highlights:

* Sanitize every parameter \(`address`es must be non null, `uint` values are non null and within expected bounds etc\)
* You must set the [CollateralJoin](https://github.com/reflexer-labs/geb-deploy/blob/master/src/AdvancedTokenAdapters.sol) contract of the specific collateral type you're targeting, as well as the [LiquidationEngine](https://github.com/reflexer-labs/geb/blob/master/src/LiquidationEngine.sol), [OracleRelayer](https://github.com/reflexer-labs/geb/blob/master/src/OracleRelayer.sol), [SAFEEngine](https://github.com/reflexer-labs/geb/blob/master/src/SAFEEngine.sol), [GebSafeManager](https://github.com/reflexer-labs/geb-safe-manager/blob/master/src/GebSafeManager.sol) and [SAFESaviourRegistry](https://github.com/reflexer-labs/geb-safe-saviours/blob/master/src/SAFESaviourRegistry.sol)
* You must set: 
  * `keeperPayout` - amount of collateral awarded to the address that initially called LiquidationEngine.liquidateSAFE
  * `minKeeperPayoutValue` - the minimum fiat value of the `keeperPayout` which makes it compelling for keepers to save the `SAFE` instead of waiting even more to liquidate it
  * `payoutToSAFESize` - how many times more collateral there must be in a `SAFE` compared to `keeperPayout`; this prevents keepers from purposefully liquidating `SAFE`s so they get a reward that is bigger than the one offered in a collateral auction
  * `defaultDesiredCollateralizationRatio` - the default CRatio that a `SAFE` will have after it's saved; this CRatio must be greater than the liquidation ratio stored in the [OracleRelayer](https://github.com/reflexer-labs/geb/blob/a49e4486682b787571475821ec66bfa025e5183f/src/OracleRelayer.sol#L60) and that is associated with `collateralToken`
* `defaultDesiredCollateralizationRatio` must be greater than zero and smaller or equal to `MAX_CRATIO`. `MAX_CRATIO` is a constant that will be smaller than `2000` \(depending on the interface and implementation\)
* When comparing a `liquidationCRatio` from the `OracleRelayer` with a desired collateralization ratio, you must first divide `liquidationCRatio` by `CRATIO_SCALE_DOWN` so you have the same scale for both numbers
* You must integrate your saviour with [GebSafeManager](https://github.com/reflexer-labs/geb-safe-manager/blob/master/src/GebSafeManager.sol) in order to take advantage of its modularity and friendlier interface compared to the core contracts \(such as `SAFEEngine`\)
* You must check that the `collateralJoin` contract is still enabled and that it handles a token with the expected amount of decimals you want
* All variables shown in the example below must be set only once in the constructor and then they must remain immutable. If governance wants to change a parameter, they must deploy a new contract and whitelist it inside `LiquidationEngine` and `SAFESaviourRegistry`

```javascript
constructor(
      address collateralJoin_,
      address liquidationEngine_,
      address oracleRelayer_,
      address safeEngine_,
      address safeManager_,
      address saviourRegistry_,
      uint256 keeperPayout_,
      uint256 minKeeperPayoutValue_,
      uint256 payoutToSAFESize_,
      uint256 defaultDesiredCollateralizationRatio_
    ) public {
        require(collateralJoin_ != address(0), "GeneralTokenBackupReserveSafeSaviour/null-collateral-join");
        require(liquidationEngine_ != address(0), "GeneralTokenBackupReserveSafeSaviour/null-liquidation-engine");
        require(oracleRelayer_ != address(0), "GeneralTokenBackupReserveSafeSaviour/null-oracle-relayer");
        require(safeEngine_ != address(0), "GeneralTokenBackupReserveSafeSaviour/null-safe-engine");
        require(safeManager_ != address(0), "GeneralTokenBackupReserveSafeSaviour/null-safe-manager");
        require(saviourRegistry_ != address(0), "GeneralTokenBackupReserveSafeSaviour/null-saviour-registry");
        require(keeperPayout_ > 0, "GeneralTokenBackupReserveSafeSaviour/invalid-keeper-payout");
        require(defaultDesiredCollateralizationRatio_ > 0, "GeneralTokenBackupReserveSafeSaviour/null-default-cratio");
        require(payoutToSAFESize_ > 1, "GeneralTokenBackupReserveSafeSaviour/invalid-payout-to-safe-size");
        require(minKeeperPayoutValue_ > 0, "GeneralTokenBackupReserveSafeSaviour/invalid-min-payout-value");

        keeperPayout         = keeperPayout_;
        payoutToSAFESize     = payoutToSAFESize_;
        minKeeperPayoutValue = minKeeperPayoutValue_;

        liquidationEngine    = LiquidationEngineLike(liquidationEngine_);
        collateralJoin       = CollateralJoinLike(collateralJoin_);
        oracleRelayer        = OracleRelayerLike(oracleRelayer_);
        safeEngine           = SAFEEngineLike(safeEngine_);
        safeManager          = GebSafeManagerLike(safeManager_);
        saviourRegistry      = SAFESaviourRegistryLike(saviourRegistry_);
        collateralToken      = ERC20Like(collateralJoin.collateral());

        uint256 scaledLiquidationRatio = oracleRelayer.liquidationCRatio(collateralJoin.collateralType()) / CRATIO_SCALE_DOWN;

        require(scaledLiquidationRatio > 0, "GeneralTokenBackupReserveSafeSaviour/invalid-scaled-liq-ratio");
        require(both(defaultDesiredCollateralizationRatio_ > scaledLiquidationRatio, defaultDesiredCollateralizationRatio_ <= MAX_CRATIO), "GeneralTokenBackupReserveSafeSaviour/invalid-default-desired-cratio");
        require(collateralJoin.decimals() == 18, "GeneralTokenBackupReserveSafeSaviour/invalid-join-decimals");
        require(collateralJoin.contractEnabled() == 1, "GeneralTokenBackupReserveSafeSaviour/join-disabled");

        defaultDesiredCollateralizationRatio = defaultDesiredCollateralizationRatio_;
    }
```

