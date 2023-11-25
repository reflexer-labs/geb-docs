# SAFE Management Contract Naming Transition

The following tables show the before and after variable names for SAFE management related contracts.

| DssCdpManager | GebSAFEManager              |
| ------------- | --------------------------- |
| vat           | safeEngine                  |
| cdpi          | safei                       |
| urns          | safes                       |
| list          | safeList                    |
| owns          | ownsSAFE                    |
| ilks          | collateralTypes             |
| first         | firstSAFEID                 |
| last          | lastSAFEID                  |
| count         | safeCount                   |
| cdpCan        | safeCan                     |
| urnCan        | handlerCan                  |
| List.prev     | List.prev                   |
| List.next     | List.next                   |
| NewCdp        | NewSafe                     |
| cdpAllowed    | safeAllowed                 |
| urnAllowed    | handlerAllowed              |
| cdpAllow      | allowSAFE                   |
| urnAllow      | allowHandler                |
| open          | openSAFE                    |
| give          | transferSAFEOwnership       |
| ilk           | collateralType              |
| frob          | modifySAFECollateralization |
| dink          | deltaCollateral             |
| dart          | deltaDebt                   |
| flux          | transferCollateral          |
| move          | transferInternalCoins       |
| quit          | quitSystem                  |
| enter         | enterSystem                 |
| shift         | moveSAFE                    |
| NaN           | protectSAFE (NEW)           |

| GetCdps     | GetSafes     |
| ----------- | ------------ |
| getCdpsAsc  | getSafesAsc  |
| getCdpsDesc | getSafesDesc |

| DssProxyActions    | GebProxyActions                                         |
| ------------------ | ------------------------------------------------------- |
| daiJoin\_join      | coinJoin\_join                                          |
| apt                | apt                                                     |
| urn                | safeHandler                                             |
| \_getDrawDart      | \_getGeneratedDeltaDebt                                 |
| vat                | safeEngine                                              |
| jug                | taxCollector                                            |
| urn                | safeHandler                                             |
| ilk                | collateralType                                          |
| dart               | deltaDebt                                               |
| \_getWipeDart      | \_getRepaidDeltaDebt                                    |
| \_getWipeAllWad    | \_getRepaidAlDebt                                       |
| transfer           | transfer                                                |
| ethJoin\_join      | ethJoin\_join                                           |
| gem                | collateral                                              |
| gemJoin\_join      | tokenCollateralJoin\_join                               |
| hope               | approveSAFEModification                                 |
| nope               | denySAFEModification                                    |
| open               | openSAFE                                                |
| give               | transferSAFEOwnership                                   |
| giveToProxy        | transferSAFEOwnershipToProxy                            |
| cdpAllow           | allowSAFE                                               |
| urnAllow           | allowHandler                                            |
| flux               | transferCollateral                                      |
| move               | transferInternalCoins                                   |
| frob               | modifySAFECollateralization                             |
| quit               | quitSystem                                              |
| enter              | enterSystem                                             |
| shift              | moveSAFE                                                |
| makeGemBag         | makeCollateralBag                                       |
| NaN                | protectSAFE (NEW)                                       |
| lockETH            | lockETH                                                 |
| safeLockETH        | safeLockETH                                             |
| lockGem            | lockTokenCollateral                                     |
| safeLockGem        | safeLockTokenCollateral                                 |
| freeETH            | freeETH                                                 |
| freeGem            | freeTokenCollateral                                     |
| exitETH            | exitETH                                                 |
| exitGem            | exitTokenCollateral                                     |
| draw               | generateDebt                                            |
| NaN                | generateDebtAndProtectSAFE (NEW)                        |
| wipe               | repayDebt                                               |
| safeWipe           | safeRepayDebt                                           |
| wipeAll            | repayAllDebt                                            |
| safeWipeAll        | safeRepayAllDebt                                        |
| lockETHAndDraw     | lockETHAndGenerateDebt                                  |
| openLockETHAndDraw | openLockETHAndGenerateDebt                              |
| NaN                | openLockETHGenerateDebtAndProtectSAFE (NEW)             |
| lockGemAndDraw     | lockTokenCollateralAndGenerateDebt                      |
| NaN                | lockTokenCollateralGenerateDebtAndProtectSAFE (NEW)     |
| openLockGemAndDraw | openLockTokenCollateralAndGenerateDebt                  |
| NaN                | openLockTokenCollateralGenerateDebtAndProtectSAFE (NEW) |
| openLockGNTAndDraw | openLockGNTAndGenerateDebt                              |
| NaN                | openLockGNTGenerateDebtAndProtectSAFE (NEW)             |
| wipeAndFreeETH     | repayDebtAndFreeETH                                     |
| wipeAllAndFreeETH  | repayAllDebtAndFreeETH                                  |
| wipeAndFreeGem     | repayDebtAndFreeTokenCollateral                         |
| wipeAllAndFreeGem  | repayAllDebtAndFreeTokenCollateral                      |

| DssProxyActionsEnd | GebProxyActionsGlobalSettlement |
| ------------------ | ------------------------------- |
| \_free             | \_freeCollateral                |
| end                | globalSettlement                |
| freeETH            | freeETH                         |
| freeGem            | freeTokenCollateral             |
| pack               | prepareCoinsForRedeeming        |
| cashETH            | redeemETH                       |
| cashGem            | redeemTokenCollateral           |
