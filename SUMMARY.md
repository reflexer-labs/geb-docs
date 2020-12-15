# Table of contents

* [Introduction to GEB](README.md)
* [Currently Deployed Systems](currently-deployed-systems.md)
* [Current System Parameters](current-system-parameters.md)
* [FAQ](faq.md)

## Contract Variables Translation <a id="contract-translation"></a>

* [Core Contracts Naming Transition](contract-translation/naming-transition.md)
* [Governance Contracts Naming Transition](contract-translation/governance-contracts-naming-transition.md)
* [SAFE Management Contract Naming Transition](contract-translation/safe-management-contract-naming-transition.md)

## System Contracts

* [Core Module](system-contracts/core/README.md)
  * [SAFE Engine](system-contracts/core/safe-engine.md)
  * [Liquidation Engine](system-contracts/core/liquidation-engine.md)
  * [Accounting Engine](system-contracts/core/accounting-engine.md)
* [Auction Module](system-contracts/auction-module/README.md)
  * [English Collateral Auction House](system-contracts/auction-module/english-collateral-auction-house.md)
  * [Fixed Discount Collateral Auction House](system-contracts/auction-module/fixed-discount-collateral-auction-house.md)
  * [Debt Auction House](system-contracts/auction-module/debt-auction-house.md)
  * [Surplus Auction House](system-contracts/auction-module/surplus-auction-house.md)
* [Oracle Module](system-contracts/oracle-module/README.md)
  * [Oracle Relayer](system-contracts/oracle-module/oracle-relayer.md)
  * [Medianizer](system-contracts/oracle-module/medianizer/README.md)
    * [DSValue](system-contracts/oracle-module/medianizer/ds-value.md)
    * [Governance Led Median](system-contracts/oracle-module/medianizer/governance-led-median.md)
    * [Chainlink Median](system-contracts/oracle-module/medianizer/chainlink-median.md)
    * [Oracle Network Median](system-contracts/oracle-module/medianizer/oracle-network-median.md)
    * [Uniswap V2 Median](system-contracts/oracle-module/medianizer/uniswap-v2-median.md)
  * [FSM](system-contracts/oracle-module/fsm/README.md)
    * [Oracle Security Module](system-contracts/oracle-module/fsm/oracle-security-module.md)
    * [Dampened Security Module](system-contracts/oracle-module/fsm/dampened-security-module.md)
    * [FSM Governance Interface](system-contracts/oracle-module/fsm/fsm-governance-interface.md)
* [Token Module](system-contracts/token-module/README.md)
  * [Token Adapters](system-contracts/token-module/token-adapters.md)
  * [System Coin](system-contracts/token-module/system-coin.md)
  * [Protocol Token](system-contracts/token-module/protocol-token.md)
  * [Protocol Token Authority](system-contracts/token-module/protocol-token-authority.md)
  * [Protocol Token Printing Permissions](system-contracts/token-module/protocol-token-printing-permissions.md)
* [Money Market Module](system-contracts/money-market-module/README.md)
  * [Tax Collector](system-contracts/money-market-module/tax-collector.md)
* [Sustainability Module](system-contracts/sustainability-module/README.md)
  * [Stability Fee Treasury](system-contracts/sustainability-module/stability-fee-treasury.md)
* [Governance Module](system-contracts/governance-module/README.md)
  * [DSPause](system-contracts/governance-module/ds-pause.md)
  * [DSProtestPause](system-contracts/governance-module/dsprotestpause.md)
* [Shutdown Module](system-contracts/shutdown-module/README.md)
  * [Global Settlement](system-contracts/shutdown-module/global-settlement.md)
  * [ESM](system-contracts/shutdown-module/esm.md)

## Proxy Infrastructure

* [DSProxy](proxy-infrastructure/ds-proxy.md)
* [Proxy Registry](proxy-infrastructure/proxy-registry.md)

## Helper Contracts

* [SAFE Manager](helper-contracts/safe-manager.md)

## GEB.js <a id="geb-js"></a>

* [Getting Started](geb-js/getting-started.md)
* [Global Settlement Guide](geb-js/geb-js-global-settlement-guide.md)
* [API Reference](geb-js/api-reference/README.md)
  * [Geb](geb-js/api-reference/geb.md)
  * [Safe](geb-js/api-reference/safe.md)
  * [Proxy Actions](geb-js/api-reference/gebproxyactions.md)
  * [Geb Admin](geb-js/api-reference/gebadmin.md)

## APIs <a id="api"></a>

* [API Endpoints](api/api-endpoints.md)

## Pyflex

* [Getting Started](pyflex/getting-started/README.md)
  * [Configuration](pyflex/getting-started/configuration.md)
  * [GEB Basics](pyflex/getting-started/basics.md)
* [SAFE Management](pyflex/safe-management/README.md)
  * [Opening a SAFE](pyflex/safe-management/opening-a-safe.md)
  * [Closing a SAFE](pyflex/safe-management/closing-a-safe.md)
* [Numerics](pyflex/numerics.md)

## Keepers

* [Overview](keepers/overview.md)
* [Collateral Auction Keeper](keepers/collateral-auction-keeper/README.md)
  * [Running in docker](keepers/collateral-auction-keeper/running-in-docker.md)
  * [Liquidations & Auctions](keepers/collateral-auction-keeper/liquidations.md)

