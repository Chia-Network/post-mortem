# chia blockchain 2.7.1 — security release retrospective

## Intro

Chia blockchain 2.7.1 shipped on May 19th 2026, about eight weeks after 2.7.0. This post mortem covers security relevant fixes and hardening that first appear in 2.7.1, plus closely related hardening from the same release cycle (including companion wallet SDK bounds published alongside that work). Everything shipped in 2.6.1 and 2.7.0 remains in 2.7.1, so upgrading from those releases still picks up the earlier wallet, peer protocol, and soft fork preparation work.

2.7.1 is a broader maintenance release than 2.7.0. Alongside that product work, a large hardening set landed in 2.7.1: weight proof validation bounds, peer handshake and WebSocket abuse resistance, mempool and wallet transaction in flight bookkeeping, SyncStore crash paths, HintStore response caps, future gossip cache bounds, early list truncation during Streamable deserialization, tighter inbound timelord and compact VDF request handling, and concurrency limits around peak and sub slot catch up work. Related wallet SDK changes also capped puzzle evaluation cost and compressed puzzle decompression size.

The issues were identified via a mix of internal security research and external bug bounty submissions.

We have no evidence that leads us to believe these issues were ever used to exploit or attack the chain, or the Chia userbase.

## Fixes implemented

### Weight proof validation

Several weight proof paths assumed well formed peer data and could throw, loop, or do disproportionate work on bad proofs:

  Overflow proof of space edge case: One overflow validation path mishandled an out of range case instead of rejecting it. 2.7.1 adds an explicit bounds check and rejects that case.
  Fewer than two sub epoch summaries: Inner validation assumed enough summaries were present before reading earlier entries. 2.7.1 fails closed when too few summaries are present.
  Unbounded challenge segments per sub epoch: Validation could process oversized segment lists for a sub epoch. 2.7.1 caps segments per sub epoch from consensus constants and rejects oversized groups.
  Wallet fork index walk: Comparing an old weight proof to a new one could walk past the end of the shorter list. 2.7.1 pairs the two lists so the shorter proof bounds the loop.
  Reconstructing reward chain sub slots: Related hardening around first challenge index checks (clearer invalid cases, optional returns instead of bare asserts) is present on the 2.7.1 line as part of the same weight proof cleanup family.

### Peer connections — handshake and WebSocket framing

  Missing handshake timeout: Inbound connections that stalled during handshake could hold resources indefinitely. 2.7.1 wraps non local / non exempt inbound handshakes in a configurable timeout and closes timed out peers cleanly. A matching outbound handshake timeout was also added for non local clients.
  Malformed WebSocket binary frames: Bad frame data could leave connections in a zombie state that still consumed connection slots. 2.7.1 ensures bad data closes the connection cleanly.

### Mempool — duplicate transaction validation race

When multiple peers delivered the same spend at once, the “already seen” check and expensive pre validation were not atomic, so concurrent workers could repeat full validation for the same transaction and churn the bounded seen cache. 2.7.1 marks a spend as in flight before pre validation, treats in flight as already including, and clears the in flight marker in a finally block so known invalid bundles stay in the seen cache without revalidation storms.

### Wallet — transaction send and acknowledgement tracking

Wallet transaction sends tracked in flight state inconsistently across initial send, reconnect resend, and RPC push paths, and acknowledgement handling could clear resend state for acknowledgements that did not match an active send. 2.7.1 centralizes send and tracking so closed peers do not get in flight markers, and only acknowledgements that match an actually in flight transaction update wallet state; unmatched acknowledgements are ignored.

### Full node — peak and sub slot catch up concurrency

  Sub slot catch up: When a peer reported an unknown previous challenge, the signage point / end of sub slot path could run a long catch up loop with no concurrency cap, unlike the already bounded peak handler. 2.7.1 applies a limited concurrency gate to that catch up work so excess requests are dropped when capacity is full, and disconnects peers that exhaust the catch up loop so connection slots are not held indefinitely.
  New peak under load: Peak handling already used a bounded semaphore, but slow peers could hold slots for a long request timeout, and inbound peak announcements could still queue heavily while slots were busy. Follow on hardening in this release cycle shortens the backtrack block request timeout used on that path and adds admission control so that when active peak slots are full and at least one outbound full node peer exists, extra inbound peak announcements are dropped rather than queued; if the node has no outbound full node peers, inbound peaks still use the bounded queue.

### Full node stability — asserts and sync state

  Unknown parent during `add_block`: A naked assert on a missing parent block record could terminate the process during sync. 2.7.1 replaces that assert with an explicit error path.
  Heaviest peak / SyncStore eviction: Peak to peer bookkeeping could drift under eviction so `get_heaviest_peak()` hit a reachable assert. 2.7.1 fixes SyncStore handling of stale peaks and empty peer entries and replaces the naked assert with an explicit nullable peak contract.
  Height map vs DB commit ordering: Updating the height map outside the same durability boundary as the block DB commit could leave on disk state hard to reconcile after an exception. 2.7.1 hardens height map capacity updates so that path does not rely on a crashing assert and keeps map growth explicit.

### Resource exhaustion — stores, caches, and parsing

  HintStore batch LIMIT: `get_coin_ids_multi` applied a per batch SQL limit instead of across the whole response, so some subscription style paths could return more coin IDs than intended. 2.7.1 reduces the remaining budget per batch and stops once the overall cap is reached.
  Future SP / EOS / IP gossip caches: Untrusted future caches used for timelord and full node gossip are capped with keyed LRU list caches and TTLs so deferred work cannot grow without bound.
  Streamable list deserialization: Handlers previously fully parsed very large lists before applying application limits. 2.7.1 adds dynamic per field list limits during deserialization so parsing stops early.
  Window based rate limits: 2.7.1 introduces a window based rate limit capability to better bound bursty peer traffic over time, building on the all peer type enforcement work from 2.7.0.

### Timelord, compact VDF, and scheduling (defence in depth)

  Inbound timelord connections: Timelords are accepted only from localhost or configured exempt networks, reducing unsolicited timelord traffic from the open internet.
  Compact VDF request handling and active request tracking: Request bookkeeping and compact VDF paths were tightened so limited concurrency cannot leak permanent entries or accept work outside the intended request lifecycle.
  Shared `PriorityThreadPoolExecutor`: Block and unfinished block validation, mempool work, and wallet protocol servicing share a priority pool so trusted / high priority work is less likely to starve behind untrusted low priority load.
  Read only snapshots for block pre validation: Pre validation uses read only snapshots so concurrent mutation cannot race the validation view of chain state.

### Wallet SDK — puzzle run cost and decompression bounds

Companion changes in the chia wallet SDK (published in the same timeframe as this release cycle) close client side gaps that the node already treated more strictly, so applications built on the SDK inherit the same resource ceilings:

  Puzzle evaluation cost: The SDK path that runs a puzzle locally previously used an effectively unbounded cost ceiling, so evaluation could consume far more CPU than the chain would ever accept for a single block. The SDK now caps that run at the consensus maximum block CLVM cost, matching what the chain allows, and fails when the limit is hit.
  Compressed puzzle decompression: Wallet puzzle payloads can arrive zlib compressed. The SDK previously decompressed without an output size cap, so decompression could allocate a very large buffer relative to the compressed input. The SDK now rejects decompression that would expand beyond a fixed maximum aligned with the existing chia blockchain wallet compression limit, and surfaces a clear too large error instead of allocating without bound.

## Timeline (all times PST) 2026 03 26 through 2026 05 19. All times approximations

  2026 03 26: 2.7.0 released.
  April 2026: Related wallet SDK cost and decompression limits land.
  2026 05 19: 2.7.1 released.
