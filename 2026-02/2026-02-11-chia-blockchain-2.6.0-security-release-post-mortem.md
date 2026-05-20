# chia blockchain 2.6.0 — security related release retrospective


## Intro

Chia blockchain 2.6.0 shipped on 2026 02 11 (chia_rs 0.35.2, clvmr 0.16.4, GUI submodule pin `92725c68`). The release is known for protocol work including the height **8,655,000** soft fork. It also shipped a set of hardening and abuse resistance improvements on the full node, in bundled Rust crates, and in related infrastructure.

This post mortem summarizes security relevant fixes and mitigations that first appeared in 2.6.0.

The issues were identified via a mix of internal security research and external bug bounty submissions. 

We have no evidence that leads us to believe these issues were ever used to exploit or attack the chain, or the Chia userbase. Prior to the activation of the fork which patched them. 

## Fixes implemented

### Transaction gossip and queue fairness

Peers advertise transactions using the `NewTransaction` message. Before 2.6.0, a peer could misrepresent the cost or fee of an advertised spend, spam low value or zero cost advertisements, and crowd out honest traffic on the node’s validation path.

2.6.0 tightens this surface by:

  Requiring advertised cost and fee to match what the node computes when it validates the spend.
  Rejecting zero cost advertisements up front.
  Scheduling work so higher **fee per cost** is favored over noisy low fee traffic.
  Using fairer queue scheduling so a single peer cannot monopolize the transaction validator.
  Letting explicitly trusted peers (for example local wallet or services) get their transactions into the queue ahead of untrusted gossip.

Together, these changes reduce **CPU exhaustion** from dishonest transaction advertisement and make “who pays” matter more than “who shouts loudest.”

### Block generator rules

Block generators are CLVM programs. The protocol has historically allowed flexible generator shapes; some of that flexibility is a long term denial of service and cost model risk.

2.6.0 includes stricter generator rules in the Rust consensus stack (for example requiring generators to follow a simpler “quoted list” shape so there is no executable CLVM at the outer level). In this release those rules are present in the codebase but gated behind a future fork height, so mainnet consensus for accepting blocks did not change the day 2.6.0 shipped. The intent is that, when the fork activates, nodes already on a current release understand the rule and stay on the same chain.

### CLVM / mempool path (mempool only in 2.6.0)

Two changes in the bundled clvm implementation affect how spends are evaluated when farmers consider transactions for inclusion in a block (mempool path). Consensus behaviour for already confirmed blocks is unchanged in this release; these are mitigations ahead of possible future consensus alignment.

  Modular exponentiation (`op_modpow`) was priced in the cost model far below its real CPU cost. A block full of such operations could take many minutes to validate on ordinary hardware while still appearing “cheap” to the cost meter. **2.6.0** rejects `op_modpow` on the mempool path so farmers do not pick those transactions into blocks they are building. A full cost model correction still depends on a future fork. We plan to release the final change with Chia 3.0 currently.

  Division related operators (`op_div`, `op_mod`, `op_divmod`) bill linearly in operand size while the underlying big integer work grows faster. 2.6.0 enforces a 2048 byte cap on the relevant operand sizes in mempool mode so pathological operands cannot be used to burn disproportionate CPU when evaluating candidate spends.

### Other hardening in 2.6.0 (brief)

The release also includes broader reliability and defence in depth work, for example stricter TLS defaults, hardening around the full node store, tighter message typing on the peer API, and validation improvements in DNS seeder handling. These reduce attack surface and operational risk even when they are not tied to a single headline flaw.

## Timeline (all times PST) early 2026 through 2026 02 11. All times approximations

  January 2026: Changes land for `NewTransaction` validation, queue behaviour, and related full node transaction handling.
  Late January 2026: Dependency pins advance to the **chia_rs** and **clvm** versions bundled with 2.6.0.
  February 2026: 2.6.0 is published. Added transaction gossip mitigations active; adding stricter rules present but fork gated for the future (no yet flagged on); also added mempool only CLVM limits for mempool validation.
