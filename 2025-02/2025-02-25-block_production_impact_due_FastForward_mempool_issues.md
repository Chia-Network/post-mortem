
# Block creation impacted due to mempool issues.

## Issue Summary

Farmers that are eligible to create the next block have a limited time (approximately 28 seconds) to create an unfinished block and via network propagation get this to a timelord that will infuse the unfinished block to construct the finished block.

Farmers will take transactions from the mempool ordered by fee per cost, and add them into the unfinished block as a way to benefit from fees attached to transactions that go to the farmer making the block.

CNI detected that while the mempool was completely full, transactions were not being included in blocks. Additionally, block production slowed down versus expected production rate. It was determined that farmers eligible to create blocks were not creating unfinished blocks in time due to several bugs in the mempool logic around fast forward spends.

Fast forward spends are a way for a singleton transaction to be able to apply to future versions of the singleton.

Fast forward spends enable offer files to refer to singletons by coin ID, even though the coin ID changes for every state update.

In addition, fast forward spends are used for vaults. Because the vault singleton is used to authorize all coins belonging to it, it would be too limiting to only be able to have one vault spend “in-flight” at a time. Fast forward allows multiple spends of the vault singleton to be in the mempool concurrently, and farmers deduplicate and rebase them sequentially as they create blocks.


CNI determined the following issues were the root cause of the block creation slow down:
* 1. Fast forward spends don’t become invalid just because their coin is spent. They can be fast-forwarded to the latest version of the singleton. Fast-forward spends rely on other parts of the spend bundle for being evicted  (e.g. that coin is spent by a block). We failed to consider the case where the only spend in a bundle was a fast-forward spend, it would not be evicted after the singleton was melted. 
* 2. FF-Spends enter the mempool even if their coin is already spent. When we form the block they will be fast-forwarded on top of the latest version of the singleton. However, we failed to validate that the singleton still existed. I.e. we would accept singleton spends into the mempool for singletons which have been melted and no longer exist.
* 3. When we formed a block, FF spends would be updated to spend the most recent coin of a singleton. This update was done in place, causing the mempool to change and propagate the fast-forwarded versions of the mempool item to the rest of the network. This would cause the mempool item to duplicate (double in numbers) every time a farmer tried to form a block.

With these 3 bugs, someone submitting a singleton spend that’s eligible for fast forward, without bundling it with any other spend would:
* Be considered valid and enter the mempool and propagate throughout the network.
* Whenever a farmer would make a block, it would multiply the spend, propagating the new transactions throughout the network (3)
* Trying to stop the spread by melting the singleton makes the transactions not go through, but the spends were not evicted from the mempool (2). In our scenario, mempool was full of these spends when this happened. All of these spends failed during block creation, causing farmers to time-out before finding any valid transactions.
* Farmers trying to create a block with the affected mempool items would experience performance issues leading them to miss the cutoff time for their block to be included in the blockchain.



## Timeline (all times PST) 02/13/2025 - 02/19/2025. All times approximations

02-13-2025 11:00 pm - Mempool spendbundle count started slowly increasing.

02-14-2025 11:30 am - Mempool cost hits max cost.

02-14-2025 1:10 pm - Mempool spendbundle count plateaus at ~6200.

02-14-2025 4:45 pm - The Chia blockchain netspace started decreasing and timelord infused blocks per minute had a marked drop.

02-14-2025 11:00 pm - The cause of the issues were diagnosed by CNI and a hotfix release of the chia blockchain software was prepared.

02-15-2025 1:30 am - The hotfix release of the Chia blockchain software build process was initiated.

02-15-2025 3:00 am - The hotfix release of the Chia blockchain software deployment completed as version 2.5.1

02-19-2025 3:15 pm - Version 2.5.2 was released that included the bug fixes for the Fast Forward issues detailed in the Issue Summary.



## Resolution and recovery

Due to the decentralized nature of the Chia blockchain with different configurations and versions distributed over many nodes, the majority of the blockchain netspace was not impacted by the Fast Forward issues.

Version 2.5.1 of the Chia blockchain contains a workaround that caps the unfinished block creation time, to ensure the unfinished block reaches a timelord in a timely manner. The blockchain netspace started recovery towards previous levels after the v2.5.1 release.

Version 2.5.2 included fixes that as a result invalidated the mempoolItems from the Fast Forward issues and the mempool was cleared of these items along with the userbase uptake of v2.5.2.
* 1. SpendBundles submitted to the mempool that only contained fast-forward spends are rejected.
* 2. When a spend is “rebased” on top of the latest version of the singleton, we don’t update the item in the mempool, risking distributing this new copy to the network.
* 3. When ingesting spend bundles into the mempool, any spend eligible for fast-forward is validated to ensure the latest version of this singleton still exists (with the given puzzle hash).