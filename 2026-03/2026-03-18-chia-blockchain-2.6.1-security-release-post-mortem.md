# chia blockchain 2.6.1 — security release retrospective
## Intro

Chia blockchain 2.6.1 shipped on 2026 03 18 (chia_rs 0.38.2, clvmr 0.17.1, GUI submodule pin `93da2237`). It is a concentrated hardening release: a large set of fixes across the Python full node and wallet, bundled chia_rs and clvm Rust crates, and the GUI. Themes include resource exhaustion, incorrect or dangerous signing behaviour, memory leaks, cross component trust boundaries, and alignment between Python and Rust when interpreting CLVM data.

Anyone who runs a wallet and accepts offers from untrusted sources should treat upgrading to at least 2.6.1 as essential. The worst issue in this bunch is wallet level: a crafted offer could previously cause the wallet to sign spends the user did not intend.

The issues were identified via a mix of internal security research and external bug bounty submissions. 

We have no evidence that leads us to believe these issues were ever used to exploit or attack the chain, or the Chia userbase. Prior to the activation of the fork which patched them. 

We also wanted to note that the Chia cloud platform was patched against the issues in this post mortem at the same time this release was published. 

## Fixes implemented

### Wallet — offer acceptance and signing

When taking an offer, the wallet RPC layer could auto sign every coin spend bundled into the merged transaction—including spends the user never agreed to that still referenced the user’s own coins. A malicious offer file could therefore trick the wallet into moving the victim’s XCH, CATs, or NFTs to an attacker controlled puzzle hash while the user believed they were only accepting a normal offer.

2.6.1 stops auto signing on the take offer path so only spends the user actually authorized are signed. If you use offers, upgrade before accepting offers from people or sites you do not fully trust.

### Peer to peer — denial of service and memory pressure

Several handlers assumed good faith about the size or shape of peer supplied data:

  Handshake “software version” string could be unbounded, causing large allocations during an early protocol phase.
  Inbound connection accounting treated only known node types; peers claiming an unrecognized type could bypass per type connection limits and open effectively unlimited connections when combined with other factors.
  Requests for additions by puzzle hash and requests for removals by coin name could carry enormous lists, tying up memory and CPU in proportion to attacker chosen list length. 2.6.1 enforces sensible caps.
  Peer list gossip could carry extremely long host strings, causing unbounded memory retention over time. 2.6.1 validates and caps those strings.

### Timelord and VDF handling

  The VDF server could leave TCP connections open when a client failed the whitelist check, leaking file descriptors until the process failed.
  Compact VDF responses could be accepted without a prior request, bypassing concurrency limits meant to cap expensive validation. 2.6.1 rejects unsolicited compact VDF responses so that path cannot be abused for CPU exhaustion.
  Infusion point caching could grow without bound because time to live was never applied. 2.6.1 restores TTL behaviour.

### Full node — correctness and crash resistance

  Pending transaction cache eviction could hit an inconsistent internal state and raise errors or evict the wrong entries under pressure.
  Long sync could hit a hard assert on malformed block batches from a peer, aborting the node process in a way that the same peer could repeat indefinitely. 2.6.1 turns that into a handled protocol error so the peer can be dropped and sync can continue.
  Cross protocol messaging: handlers previously did not consistently verify that the declared peer type (wallet, full node, farmer, and so on) was allowed to invoke a given RPC style message. 2.6.1 enforces those boundaries so, for example, a wallet connection cannot drive full node only code paths that assume a different trust model.

### Data layer and plugins

  HTTP calls from upload paths could hang indefinitely without a timeout, stalling the data layer event loop. 2.6.1 adds explicit timeouts.
  An S3 related upload path could log a validation failure for a mismatched store identifier but still fall through and upload anyway; 2.6.1 corrects the control flow so invalid cases do not upload.

### Wallet — offer summaries and CPU

Building a human readable summary of an offer runs CLVM for offered coins. That path previously had no cost budget, so a malicious offer could burn arbitrary CPU during summary generation. 2.6.1 runs that work under a bounded cost budget and rejects offers that exceed it.

### Keyring and passphrase prompts

A logic bug could make the software believe a master passphrase was already cached when it was not, skipping prompts when the user should have been asked. 2.6.1 fixes the comparison so behaviour matches user expectations.

### Rust (`chia_rs`) — CLVM integer decoding

Integer fields decoded from CLVM atoms could be interpreted differently in Rust than in Python (truncation and signedness edge cases). That class of bug is a consensus risk if both implementations must agree. 2.6.1 tightens decoding to a strict, consistent behaviour.

### CLVM (`clvm`) — mempool stricter integers and block build cost

  The softfork operator’s cost argument could use non canonical integer encodings (for example with leading zeros) such that extra CPU was spent scanning bytes that were not fully reflected in the billed cost. 2.6.1 rejects those encodings in mempool mode so farmers do not include such spends; consensus enforcement of the same rule will be a hard fork tied to the release of Chia 3.0 as this enforcement changes consensus rules. 
  Block creation / serialization had super linear behaviour for certain input shapes, which could make farmers miss time windows when the mempool contained adversarially shaped cheap spends. 2.6.1 ships improvements on the clvm side; some complementary work in other components was still in flight at the 2.6.1 cut, users will need 2.7.0 to fully close this issue. 

### GUI — offer notification URLs

The GUI’s offer notification feature validated external domains using a suffix match, which allowed lookalike domains to pass as whitelisted sites and could leak metadata or load unexpected content. 2.6.1 tightens validation to match the intended site more strictly. Stronger edge case handling landed in a later release.

### Fork related constants

2.6.1 introduces height constants for several future soft fork rules (generator shape, canonical integers, block references, canonical serialization). Those rules are not active on mainnet at the 2.6.1 release; the constants prepare the codebase for coordinated activation later.

## Timeline (all times PST) 2026 02 11 through 2026 03 18. All times approximations

  2026 02 11: 2.6.0 released.
  February–March 2026: Hardening and correctness fixes are integrated and tested on the release branch.
  2026 03 18: 2.6.1 is tagged and published with updated dependency and GUI pins.
