# Unfinished_blocks with different Foliage.

## Issue Summary

When a farmer runs multiple (redundant) full node and farmer instances, harvesting the same plots, it's possible to produce multiple different unfinished blocks with the same proof-of-space. Full nodes may have slightly different views of the mempool, because of quirks in transaction  gossip. Therefore nodes may produce slightly different blocks. As an example, we consider a new transaction being propagated throughout the network that has reached one node but not the other when the block is created.

Since the network sees these blocks as equally valid (because the reward block hash, i.e. proof-of-space) is the same, they will each propagate to nodes and which one is seen by which node is non deterministic because of the previously mentioned quirks in gossiping of transactions and blocks. This is a system designed to be unpredictable in an effort to provide defense against sybil type attacks. In a system with many competing timelords, different timelords are likely to infuse different versions of the unfinished blocks. Often timelords will not  be aware of the existence of the other variants of the chain, because it's never propagated to their part of the network.

These blocks, once infused, will form competing chains, eventually resolved by one side of the network re-orging to the other chain.


## Timeline (all times PST) 11/20/2023 - 02/28/2024. All times approximations

11-20-2023 - A researcher submitted a bug report via bugcrowd (https://bugcrowd.com/chia-network-bb) detailing the issue.

11-21-2023 - The ASIC timelords that sit geographically far apart were linked together by their full_nodes to ensure they see the same variant first, and only infuse that variant.

01-26-2024 - A fix was introduced to the codebase at https://github.com/Chia-Network/chia-blockchain/pull/17247

02-28-2024 - Version 2.2.0 was released that included the fix.


## Resolution and recovery

The immediate resolution was to ensure the fastest timelords on the Chia network, currently the ASIC timelords, were linked together by their fullnodes.
This ensured that if multiple unfinished_block variants (each with different Foliage) were propagating on the network, the timelords would see the same variant first
and ignore the other variants.

The fix in the codebase allows for the multiple variants (up to 3) to reach the timelords, where the timelord will infuse only one (the one with the lowest foliage transaction block hash).

For more information about the issue and resolution, visit:
https://github.com/Chia-Network/chia-blockchain/pull/17247
