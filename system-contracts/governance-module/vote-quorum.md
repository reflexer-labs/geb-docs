---
description: Approval voting using protocol tokens
---

# Vote Quorum

## 1. Summary <a id="1-introduction-summary"></a>

This contract provides a way to elect a lead candidate \(proposal\) via approval voting. This may be combined with another contract, such as `DSAuthority`, to elect a ruleset for a smart contract system.

## 2. Contract Variables & Functions

* `ballots` - a mapping of `bytes32` to `address` arrays. Represents sets of candidates. Weighted votes are given to ballots
* `votes` - a mapping of voter addresses to the ballot they have voted for
* `approvals` - a mapping of candidate addresses to their `uint256` weight
* `deposits` - a mapping of voter addresses to `uint256` tokens locked
* `PROT` - `DSToken` used for voting
* `IOU` - `DSToken` issued in exchange for locking `PROT` tokens
* `votedAuthority` - contains the address of the current authority \(address\) that received the most votes
* `MAX_CANDIDATES_PER_BALLOT` - maximum number of candidates a ballot can hold
* `addVotingWeight(wad: uint256)` - charges the user `wad` `PROT` tokens, issues an equal amount of `IOU` tokens to the user, and adds `wad` weight to the candidates on the user's selected ballot
* `removeVotingWeight(wad: uint256)` - charges the user `wad` `IOU` tokens, issues an equal amount of `PROT` tokens to the user, and subtracts `wad` weight from the candidates on the user's selected ballot
* `groupCandidates(candidates: address[])` - save a set of ordered addresses and return a unique identifier for it
* `vote(candidates: address[])` - saves a set of ordered addresses as a ballot, moves the voter's weight from their current ballot to the new ballot, and returns the ballot's identifier
* `vote(ballot: bytes32)` - removes voter's weight from their current ballot and adds it to the specified ballot
* `electCandidate(whom: address)` - checks the given address and promotes it to `votedAuthority`

   if it has more weight than the current candidate

* `setOwner(owner: address)` - reverts the transaction. Overridden from `DSAuth`
* `setAuthority(DSAuthority authority_)` - reverts the transaction. Overridden from `DSAuth`
* `setRootUser(who: address, enabled: bool)` - reverts the transaction. Overridden from `DSRoles`
* `isUserRoot(address who) public view returns (bool)` - returns `true` if the given address is the root user

**Events**

* `GroupCandidates` - emitted when someone calls `groupCandidates` to batch multiple addresses together and return an id for the batch. Contains:
  * `ballot` - batch id

## 3. Walkthrough <a id="2-contract-details"></a>

Voters lock up voting tokens to give their votes weight and in return receive IOU tokens which are useful for secondary governance mechanisms. The IOU tokens may not be exchanged for the locked tokens except by someone who has actually locked funds in the contract, and only up to the amount they have locked.

The IOU token allows for chaining governance contracts. An arbitrary number of `VoteQuorum` or other contracts of its kind may essentially use the same governance token by accepting the IOU token of the `VoteQuorum` contract before it as a governance token. E.g. given three `VoteQuorum` contracts, `voteQuorumA`, `voteQuorumB`, and `voteQuorumC`, with `voteQuorumA.PROT` being the protocol token, setting `voteQuorumB.PROT` to `voteQuorumA.IOU` and `voteQuorumC.PROT` to `voteQuorumB.IOU` allows all three contracts to essentially run using a common pool of tokens.

**Approval voting** is when each voter selects which candidates they approve of, with the top `n` "most approved" candidates being elected. Each voter can cast up to `n + k` votes, where `k` is some non-zero positive integer. This allows voters to move their approval from one candidate to another without needing to first withdraw support from the candidate being replaced. Without this, moving approval to a new candidate could result in a less-approved candidate moving momentarily into the set of elected candidates.

In the case of `ds-vote-quorum`, `n` is 1.

In addition, `ds-vote-quorum` weights votes according to the quantity of a voting token they've chosen to lock up in the `VoteQuorum` or `VoteQuorumApprovals` contract.

It's important to note that the voting token used in a `ds-vote-quorum` deployment must be specified at the time of deployment and cannot be changed afterward.

{% hint style="info" %}
**Notice for Developers**

If you are writing a frontend for this smart contract, please note that the `address[]` parameters passed to the `groupCandidates` and `vote` functions must be _byte-ordered sets_. E.g., `[0x0, 0x1, 0x2, ...]` is valid, `[0x1, 0x0, ...]` and `[0x0, 0x0, 0x1, ...]` are not. This ordering constraint allows the contract to cheaply ensure voters cannot multiply their weights by listing the same candidate on their ballot multiple times.
{% endhint %}

