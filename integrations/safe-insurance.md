---
description: Building and integrating insurance contracts for SAFEs
---

# SAFE Insurance

## 1. Overview

The GEB [LiquidationEngine](https://github.com/reflexer-labs/geb/blob/master/src/LiquidationEngine.sol) allows governance to [whitelist](https://github.com/reflexer-labs/geb/blob/a49e4486682b787571475821ec66bfa025e5183f/src/LiquidationEngine.sol#L88) external insurance contracts for SAFEs. SAFE users can [attach](https://github.com/reflexer-labs/geb/blob/a49e4486682b787571475821ec66bfa025e5183f/src/LiquidationEngine.sol#L290) insurance contracts to their positions and this way have an extra layer of protection against liquidation.

Anyone can build and propose new insurance contracts, assuming that the contracts abide by the requirements and principles outlined below. A central repository with SAFE insurance contracts \(also called saviours\) and interfaces can be found [here](https://github.com/reflexer-labs/geb-safe-saviours).

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
    function getKeeperPayoutValue() virtual public returns (uint256);
    function keeperPayoutExceedsMinValue() virtual public returns (bool);
    function canSave(address) virtual external returns (bool);
    function tokenAmountUsedToSave(address) virtual public returns (uint256);
}
```



