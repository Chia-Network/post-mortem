# chia blockchain 2.7.1 — security release retrospective

## Intro

Chia blockchain 2.7.1 shipped on May 19th 2026, about eight weeks after 2.7.0. This post mortem covers security relevant fixes and hardening that first appear in 2.7.1. Everything shipped in 2.6.1 and 2.7.0 remains in 2.7.1, so upgrading from those releases still picks up the earlier wallet, peer protocol, and soft fork preparation work.

2.7.1 is a broader maintenance release than 2.7.0. Alongside that product work, a large hardening set landed via the 2.7.1, weight proof validation bounds, peer handshake and WebSocket abuse resistance, mempool transaction deduplication under concurrency, SyncStore crash paths, HintStore response caps, future gossip cache bounds, early list truncation during Streamable deserialization, and tighter inbound timelord and compact VDF request handling.

The issues were identified via a mix of internal security research and external bug bounty submissions.

We have no evidence that leads us to believe these issues were ever used to exploit or attack the chain, or the Chia userbase.

## Fixes implemented

### Weight proof validation

Several weight proof paths assumed well formed peer data and could throw, loop, or do disproportionate work on crafted proofs:

  Overflow proof of space at sub slot index 0: Validation used `segment.sub_slots[idx - 1]` on the overflow path. When `idx == 0`, Python negative indexing silently read the last element instead of rejecting the segment. 2.7.1 adds an explicit bounds check and rejects that case.
  Fewer than two sub epoch summaries: Inner validation indexed `summaries[-2]` without requiring `len(summaries) >= 2`. 2.7.1 fails closed when too few summaries are present.
  Unbounded challenge segments per sub epoch: Validation processed attacker sized `sub_epoch_segments` lists while only sampling a subset. 2.7.1 caps segments per sub epoch from consensus constants and rejects oversized groups.
  Wallet fork index walk: Comparing an old weight proof to a new one indexed `old_wp.sub_epochs[idx]` without staying inside the shorter list. 2.7.1 walks with `zip` so the shorter proof bounds the loop.
  Reconstructing reward chain sub slots: Related hardening around first challenge index checks (rejecting falsy index mistakes, optional returns instead of bare asserts) is present on the 2.7.1 line as part of the same weight proof cleanup family.

### Peer connections — handshake and WebSocket framing

  Missing handshake timeout (Slowloris style): Inbound peers could start a connection and stall during handshake, holding resources. 2.7.1 wraps non local / non exempt inbound handshakes in a configurable timeout and closes timed out peers with an invalid handshake path. A matching outbound handshake timeout was also added for non local clients.
  Malformed WebSocket binary frames: Bad frame data could leave connections in a zombie state that still consumed connection slots. 2.7.1 ensures bad data closes the connection cleanly.

### Mempool — duplicate transaction validation race

When multiple peers delivered the same spend at once, the “already seen” check and expensive pre validation were not atomic. Concurrent workers could each run full validation for the same transaction and churn the bounded seen cache. 2.7.1 marks a spend as in flight before pre validation, treats in flight as already including, and clears the in flight marker in a finally block so known invalid bundles stay in the seen cache without revalidation storms.

### Full node stability — asserts and sync state

  Unknown parent during `add_block`: A naked assert on a missing parent block record could kill the process during sync if a peer sent a disconnected block. 2.7.1 replaces that assert with an explicit error path.
  Heaviest peak / SyncStore eviction: FIFO eviction of `peak_to_peer` could leave `peer_to_peak` stale so `get_heaviest_peak()` hit a reachable assert under peer flooding. 2.7.1 fixes SyncStore handling of stale peaks and empty peer entries and replaces the naked assert with an explicit `Peak | None` contract.
  Height map vs DB commit ordering: Updating the height map outside the same durability boundary as the block DB commit could leave on disk state hard to reconcile after an exception. 2.7.1 hardens height map capacity updates so that path does not rely on a crashing assert and keeps map growth explicit.

### Resource exhaustion — stores, caches, and parsing

  HintStore batch LIMIT: `get_coin_ids_multi` applied `LIMIT max_items` per SQL batch instead of across the whole response, so `RegisterForPhUpdates` style paths could return far more coin IDs than intended. 2.7.1 reduces the remaining budget per batch and stops once `max_items` is reached.
  Future SP / EOS / IP gossip caches: Untrusted future caches used for timelord and full node gossip are capped with keyed LRU list caches and TTLs so deferred work cannot grow without bound under noisy peers.
  Streamable list deserialization: Handlers previously fully parsed enormous lists (for example coin ID or puzzle hash batches) before applying application limits. 2.7.1 adds dynamic per field list limits during deserialization so parsing stops early and seeks past remaining fixed size elements.
  Window based rate limits: 2.7.1 introduces a window based rate limit capability to better bound bursty peer traffic over time, building on the all peer type enforcement work from 2.7.0.

### Timelord, compact VDF, and scheduling (defence in depth)

  Inbound timelord connections: Timelords are accepted only from localhost or configured exempt networks, reducing unsolicited timelord traffic from the open internet.
  Compact VDF request handling and active request tracking: Request bookkeeping and compact VDF paths were tightened so limited concurrency cannot leak permanent entries or accept work outside the intended request lifecycle.
  Shared `PriorityThreadPoolExecutor`: Block and unfinished block validation, mempool work, and wallet protocol servicing share a priority pool so trusted / high priority work is less likely to starve behind untrusted low priority load.
  Read only snapshots for block pre validation: Pre validation uses read only snapshots so concurrent mutation cannot race the validation view of chain state.

## Timeline (all times PST) 2026 03 26 through 2026 05 19. All times approximations

  2026 03 26: 2.7.0 released.
  2026 05 19: 2.7.1 released.
