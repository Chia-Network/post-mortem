# v2.2.0 unfinished_block cache retrieval issue.

## Issue Summary

If multiple farmers share plots and win, they will each produce a new unfinished_block with the same reward block hash.

A new feature in 2.2.0 allows the network to propagate all unfinished_block variants (up to a limit) and pick the "best" one for infusion creating one full_block.

When a node receives notification from a peer of a new_peak it requests the full_blocks it doesn’t have to sync to that peak height. The node will not ask for the block generator of the full_block when it already has the unfinished_block (with the needed block generator) in its cache as an optimization.

The unfinished_block is pulled from the cache by its reward hash, and its cache insertion order determines which one is selected if there were multiple variants (with the same reward hash) of the unfinished_blocks in the cache. 

This would result in the wrong item being pulled from the cache in some scenarios, causing the reconstructed full_block to include the wrong block generator. In that case validation of the full_block would fail and the peer that provided the full_block would get banned for 10 seconds.
In addition the wrong execution results might get pulled from the cache as well due to the same issue, resulting in failed validation and peer bans.


## Timeline (all times PST) 02/26/2024 - 03/06/2024. All times approximations

02-26-2024 - `INVALID_TRANSACTIONS_FILTER_HASH` block validation was reported for the Release Candidate version in Chia’s Discord Server by beta testers.

02-28-2024 - Version 2.2.0 was released to the public.

02-29-2024 - The cache retrieval issue was confirmed and v2.2.0 was recalled.

03-06-2024 - Version 2.2.1 was released that included the fix. 


## Resolution and recovery

A fix was introduced in v2.2.1 to make the unfinished_block selection from the cache deterministic by not only pulling the cached item by its reward block hash, but also taking the foliage hash into account when constructing the full_block.

Version 2.2.0 with the cache retrieval issue was pulled, and version 2.2.1 that has the cache lookup fix was released as a replacement.

For more information about the issue and resolution, visit:
https://github.com/Chia-Network/chia-blockchain/pull/17622
