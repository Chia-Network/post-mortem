# chia blockchain 2.7.0 — security release retrospective

## Intro

Chia blockchain 2.7.0 shipped on 2026 03 26 (chia_rs 0.41.1, clvmr 0.17.4, GUI submodule pin `8a690532`), about one week after 2.6.1. This post mortem covers fixes and hardening that first appear in 2.7.0. Everything shipped in 2.6.1 remains in 2.7.0 including the wallet offer signing correction so upgrading directly from 2.6.0 to 2.7.0 still picks up the full 2.6.1 wallet and node hardening set.

2.7.0 is intentionally narrow and follow up shaped: more peer protocol tightening, a few high leverage correctness bugs, data layer bounds, Rust side amount parsing consistency, BLS related operator performance work bundled with the clvm dependency, preparation for near term soft fork activation of rules that were previously coded only against a far future height, and one additional mempool side CLVM limit.

The issues were identified via a mix of internal security research and external bug bounty submissions. 

We have no evidence that leads us to believe these issues were ever used to exploit or attack the chain, or the Chia userbase. Prior to the activation of the fork which patched them. 

## Fixes implemented

### Peer to peer — unsolicited work and rate limits

  `respond_transaction`: A peer could send a full transaction response even when the node never asked for that transaction, triggering full validation and optional relay. 2.7.0 only accepts responses that match an outstanding request, similar in spirit to the compact VDF “no unsolicited responses” hardening from 2.6.1.
  Rate limits on non full node peers: Rate limit violations were computed for all peer types but disconnection and ban enforcement effectively only ran for full nodes. Wallets and other roles could be flooded while only logging over limit traffic. 2.7.0 enforces disconnect/ban for abusive peers across all node types (with the usual localhost and configured exemptions).
  Unknown protocol message types: Garbage or unknown type bytes could slip through code paths that treated them as “allowed,” avoiding rate limit accounting. 2.7.0 routes unknown types through the same strict handling used for malformed protocol data.
  Log injection via handshake version string: The peer supplied software version string was written to logs verbatim, allowing newline or escape sequences to confuse operators or tooling. 2.7.0 sanitizes the field before logging.
  Coin subscription registration: Building a Python set from the entire incoming coin ID list before applying size limits could allocate heavily and stall the event loop on huge requests. 2.7.0 avoids that eager allocation.

### Timelord — invalid VDF proofs

After failed VDF validation, one code path logged the failure but then fell through and still announced the proof to connected full nodes, wasting network wide CPU re validating junk. 2.7.0 adds the missing control flow so invalid proofs are not broadcast.

### Full node — stability and memory

  Wallet registration during startup: Assertions could crash the full node process if a wallet peer registered before the node finished initializing its peak state (including during reorgs). 2.7.0 returns a clean error to the peer instead of terminating the node.
  Compact VDF request tracking: When a concurrency limit rejected new work, bookkeeping could leave permanent entries in an internal request set, causing a slow memory leak under sustained abuse. 2.7.0 removes those entries on the rejection path.

### Data layer

  Delta / tree file parsing read record sizes from disk with no upper bound, so a corrupt or malicious file could request multi gigabyte allocations. 2.7.0 adds maximum size guards appropriate to the format.
  Plugin based downloads could hang the event loop when the plugin’s HTTP client had no timeout. 2.7.0 applies consistent client timeouts.

### Rust (`chia_rs`) — amount parsing consistency

Different helper functions that parse coin amounts from the same block could disagree on edge cases (for example unusually encoded lengths). That is dangerous for consensus alignment between code paths. 2.7.0 standardizes parsing so sibling “trusted block” helpers agree.

### CLVM (`clvm`) — BLS operator cost gap (stage one)

Point operations on BLS curves (`g1_negate`, `g2_negate`, and related primitives) were cheap in the cost model relative to real decompression and validation work, so an attacker could pack many expensive operations into a single block’s CLVM budget. 2.7.0 ships implementation optimizations in the bundled clvm release (caching successful element decodes in the allocator so repeated operations on the same points become cheaper). Changing the numeric consensus CLVM cost for those opcodes (so every node bills the same amount on chain) is a separate change and a hard fork item for Chia 3.0; 2.7.0 narrows the real versus billed gap in practice but does not, by itself, change the consensus cost on mainnet.

### Soft fork rules (2.7.0 client vs network timeline)

Several hardening rules (for example: canonical integer encodings for certain arguments, disallowing non empty block reference lists past the activation height, stricter generator shape, stricter generator serialization) were staged in code under older releases, initially pointed at a distant height after the hopeful launch date of 3.0, then re-targeted in 2.7.0 to height 8,655,000 

This write up is intended to ship after that fork is live. On mainnet, enforcement begins at the height 8,655,000, not when an individual user installs 2.7.0. Running 2.7.0 or a compatible later release (likely Chia 3.0) keeps a node aligned with rules the chain already applies.

### CLVM — mempool operand limits (multiply and divide family)

Building on mempool only operand caps for division from 2.6.0, 2.7.0 adds a broader mempool “limits” mode for multiplication and the same division family: oversized integer or scalar operands for those operators are rejected for candidate mempool spends. Consensus behaviour for already confirmed blocks is unchanged in this release; this is additional farmer side protection ahead of any future consensus tightening.

## Timeline (all times PST) mid March 2026 through 2026 03 26. All times approximations

  2026 03 18: 2.6.1 released.
  Mid March 2026: Python, chia_rs, and clvm changes converge toward the pins shipped in 2.7.0.
  2026 03 26: 2.7.0 tagged and published.
