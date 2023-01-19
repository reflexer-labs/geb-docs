# Steps to deploy a RAI fork.

This document outlines the steps to deploy an instance of the GEB Protocol. It's goal is not to provide extensive information on each of the steps (the readme files on each repository have detailed information) but to be a general guide on the steps to run a successful fork and the repositories where the code of the several parts of the protocol reside.

## 1. Smart contracts
Smart contracts are in several repositories in the [reflexer-labs](https://github.com/reflexer-labs) organization, the main one being the [GEB](https://github.com/reflexer-labs/geb) repo.

To deploy the protocol, to repositories are used:
- [geb-deploy](https://github.com/reflexer-labs/geb-deploy): Solidity contracts used for deploying the protocol
- [geb-deployment-scripts](https://github.com/reflexer-labs/geb-deployment-scripts): Bash script the deploys the full system, allows for customization of the setup (refer to readme.md on the repository for more info).

## 2. Pinger Bots / Keepers

The protocol requires external actors to call certain functions to maintain the system alive. These range from updating price feeds to recomputing the redemption rate. The calls are incentivized by the system (paying rewards from the stability fee treasury to callers) but initially the deployer needs to call them, until the stability fee treasury is funded and rewards are configured. The bots live in the [geb-pinger-bots](https://github.com/reflexer-labs/geb-pinger-bots) repository.

The system also relies on keepers to buy into auctions for collateral, and the less common surplus and debt auctions. The keeper bots are in the repository [auction-keeper](https://github.com/reflexer-labs/auction-keeper).

## 3. Graph

The fronted relies on both on the Graph and RPC calls, the subgraph is kept at [geb-subgraph](https://github.com/reflexer-labs/geb-subgraph).

## 4. Frontend

The frontend is maintained at the repository [geb-app](https://github.com/reflexer-labs/geb-app).

## 5. Stats

[This](https://stats.reflexer.finance/) stats page is maintained in the [geb-stats](https://github.com/reflexer-labs/geb-stats) repo.

## 6. Libs / Console

The repositories above make use of [geb.js](https://github.com/reflexer-labs/geb.js). One can interact with a system deployment through the terminal using [geb-console](https://github.com/reflexer-labs/geb-console).