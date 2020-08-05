# Governance Contracts Naming Transition

The following tables show the before and after variable names for MCD governance related contracts.

| DSPause | DSPause |
| :--- | :--- |
| wait | isDelayed |
| setOwner | setOwner |
| setAuthority | setAuthority |
| setDelay | setDelay |
| plans | scheduledTransactions |
| proxy | proxy |
| delay | delay |
| hash | getTransactionDataHash |
| soul | getExtCodeHash |
| plot | scheduleTransaction |
| drop | abandonTransaction |
| exec | executeTransaction |
| eta | earliestExecutionTime |
| fax | parameters |
| tag | codeHash |

| DSChief | VoteQuorum |
| :--- | :--- |
| slates | ballots |
| votes | votes |
| approvals | approvals |
| deposits | deposits |
| GOV | PROT |
| IOU | IOU |
| hat | votedAuthority |
| MAX\_YAYS | MAX\_CANDIDATES\_PER\_BALLOT |
| Etch | GroupCandidates |
| lock | addVotingWeight |
| free | removeVotingWeight |
| etch | groupCandidates |
| vote | vote |
| lift | electCandidate |
| addWeight | addWeight |
| subWeight | subWeight |
| requireByteOrderedSet | requireByteOrderedSet |
| setOwner | setOwner |
| setAuthority | setAuthority |
| isUserRoot | isUserRoot |
| setRootUser | setRootUser |

| DSAuth | DSAuth |
| :--- | :--- |
| authority | authority |
| owner | owner |
| setOwner | setOwner |
| setAuthority | setAuthority |
| auth | auth |
| isAuthorized | isAuthorized |

| DSRoles | DSRoles |
| :--- | :--- |
| \_root\_users | \_root\_users |
| \_user\_roles | \_user\_roles |
| \_capability\_roles | \_capability\_roles |
| \_public\_capabilities | \_public\_capabilities |
| getUserRoles | getUserRoles |
| getCapabilityRoles | getCapabilityRoles |
| isUserRoot | isUserRoot |
| isCapabilityPublic | isCapabilityPublic |
| hasUserRole | hasUserRole |
| canCall | canCall |
| BITNOT | BITNOT |
| setRootUser | setRootUser |
| setUserRole | setUserRole |
| setPublicCapability | setPublicCapability |
| setRoleCapability | setRoleCapability |

| MkrAuthority | ProtocolTokenAuthority |
| :--- | :--- |
| root | root |
| NaN | owner \(NEW\) |
| sudo | isRootCalling |
| NaN | isRootOrOwnerCalling \(NEW\) |
| LogSetRoot | LogSetRoot |
| NaN | LogSetOwner \(NEW\) |
| setRoot | setRoot |
| NaN | setOwner \(NEW\) |
| wards | authorizedAccounts |
| LogRely | LogAddAuthorizedAccount |
| rely | addAuthorization |
| LogDeny | LogRemoveAuthorizedAccount |
| deny | removeAuthorization |
| burn | burn |
| burnFrom | burnFrom |
| mint | mint |
| canCall | canCall |

| VoteProxy | VoteProxy |
| :--- | :--- |
| cold | cold |
| hot | hot |
| gov | gov |
| iou | iou |
| chief | voteQuorum |
| auth | isAuthorized |
| lock | addVotingWeight |
| free | removeVotingWeight |
| freeAll | removeAllVotingWeight |
| vote | vote |

| DSGuard | DSGuard |
| :--- | :--- |
| LogPermit | LogPermit |
| LogForbid | LogForbid |
| ANY | ANY |
| canCall | canCall |
| permit | permit |
| forbid | forbid |

| DSStop | DSStop |
| :--- | :--- |
| stopped | stopped |
| stoppable | stoppable |
| stop | stop |
| start | start |

