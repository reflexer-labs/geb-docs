# CDP Management Contract Naming Transition

The following tables show the before and after variable names for CDP management related contracts.

| DssCdpManager | GebCDPManager |
| :--- | :--- |
| vat | cdpEngine |
| cdpi | cdpi |
| urns | cdps |
| list | cdpList |
| owns | ownsCDP |
| ilks | collateralTypes |
| first | firstCDPID |
| last | lastCDPID |
| count | cdpCount |
| cdpCan | cdpCan |
| urnCan | handlerCan |
| List.prev | List.prev |
| List.next | List.next |
| NewCdp | NewCdp |
| cdpAllowed | cdpAllowed |
| urnAllowed | handlerAllowed |
| cdpAllow | allowCDP |
| urnAllow | allowHandler |
| open | openCDP |
| give | transferCDPOwnership |
| ilk | collateralType |
| frob | modifyCDPCollateralization |
| dink | deltaCollateral |
| dart | deltaDebt |
| flux | transferCollateral |
| move | transferInternalCoins |
| quit | quitSystem |
| enter | enterSystem |
| shift | moveCDP |
| NaN | protectCDP |

| GetCdps | GetCdps |
| :--- | :--- |
| getCdpsAsc | getCdpsAsc |
| getCdpsDesc | getCdpsDesc |

| DssProxyActions | GebProxyActions |
| :--- | :--- |
| daiJoin\_join | coinJoin\_join |
| apt | apt |
| urn | cdpHandler |
| \_getDrawDart | \_getGeneratedDeltaDebt |
| vat | cdpEngine |
| jug | taxCollector |
| urn | cdpHandler |
| ilk | collateralType |
| dart | deltaDebt |
| \_getWipeDart | \_getRepaidDeltaDebt |
| \_getWipeAllWad | \_getRepaidAlDebt |
| transfer | transfer |
| ethJoin\_join | ethJoin\_join |
| gem | collateral |
| gemJoin\_join | tokenCollateralJoin\_join |
| hope | approveCDPModification |
| nope | denyCDPModification |
| open | openCDP |
| give | transferCDPOwnership |
| giveToProxy | transferCDPOwnershipToProxy |
| cdpAllow | allowCDP |
| urnAllow | allowHandler |
| flux | transferCollateral |
| move | transferInternalCoins |
| frob | modifyCDPCollateralization |
| quit | quitSystem |
| enter | enterSystem |
| shift | moveCDP |
| makeGemBag | makeCollateralBag |
| NaN | protectCDP \(NEW\) |
| lockETH | lockETH |
| safeLockETH | safeLockETH |
| lockGem | lockTokenCollateral |
| safeLockGem | safeLockTokenCollateral |
| freeETH | freeETH |
| freeGem | freeTokenCollateral |
| exitETH | exitETH |
| exitGem | exitTokenCollateral |
| draw | generateDebt |
| NaN | generateDebtAndProtectCDP \(NEW\) |
| wipe | repayDebt |
| safeWipe | safeRepayDebt |
| wipeAll | repayAllDebt |
| safeWipeAll | safeRepayAllDebt |
| lockETHAndDraw | lockETHAndGenerateDebt |
| openLockETHAndDraw | openLockETHAndGenerateDebt |
| NaN | openLockETHGenerateDebtAndProtectCDP \(NEW\) |
| lockGemAndDraw | lockTokenCollateralAndGenerateDebt |
| NaN | lockTokenCollateralGenerateDebtAndProtectCDP \(NEW\) |
| openLockGemAndDraw | openLockTokenCollateralAndGenerateDebt |
| NaN | openLockTokenCollateralGenerateDebtAndProtectCDP \(NEW\) |
| openLockGNTAndDraw | openLockGNTAndGenerateDebt |
| NaN | openLockGNTGenerateDebtAndProtectCDP \(NEW\) |
| wipeAndFreeETH | repayDebtAndFreeETH |
| wipeAllAndFreeETH | repayAllDebtAndFreeETH |
| wipeAndFreeGem | repayDebtAndFreeTokenCollateral |
| wipeAllAndFreeGem | repayAllDebtAndFreeTokenCollateral |

| DssProxyActionsEnd | GebProxyActionsGlobalSettlement |
| :--- | :--- |
| \_free | \_freeCollateral |
| end | globalSettlement |
| freeETH | freeETH |
| freeGem | freeTokenCollateral |
| pack | prepareCoinsForRedeeming |
| cashETH | redeemETH |
| cashGem | redeemTokenCollateral |

