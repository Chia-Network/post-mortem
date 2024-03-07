# Inefficient Mempool Item selection.

## Issue Summary

Blocks were not filled with transactions to capacity.

Chia's consensus allows for 11 Billion in CLVM cost to be added to a block in transactions, where the CLVM cost for each transaction depends on their complexity, coins involved etc.
When a farmer creates a block with transactions, it will select them from their mempool in order of `fee per cost`.

The mempool item selection algorithm fills a new block by repeatedly selecting the highest-priority transaction from the mempool.
When the next transaction would overfill the block, the node would consider the block finished and submit it to the network.

This was inefficient in some cases. For example, if a block was less than half full, but the next transaction in the queue was large enough to overfill the block, the node would stop looking for more transactions. The resulting block would be filled to nowhere near capacity, even though it could have held many smaller transactions.


The following situation occurred:
- a transaction was in the mempool with CLVM cost that was close in size to the mempool selection algorithm maximum. 
- the transaction's `fee per cost` was not high enough for the mempool selection algorithm to add it first, yet reasonably high to have it inspected soon after.

As a result the mempool selection algorithm would select other items first, and detecting the "stuck" large item would exceed the maximum stop there.
This resulted in blocks to contain only a few transactions and the "stuck" transaction to not be included and cause the same issue in the subsequent blocks.


## Timeline (all times PST) 01/16/2024 - 02/28/2024. All times approximations

01-16-2024 - Chia detected sub optimal transaction inclusion in blocks due to the "stuck" transaction.

01-16-2024 - A Chia farmer of reasonable size was asked to include the "stuck" transaction in a block, and the farmer created such a block.

01-25-2024 - A better selection algorithm was introduced to the codebase at https://github.com/Chia-Network/chia-blockchain/pull/17346

02-28-2024 - Version 2.2.0 was released that included the fix.


## Resolution and recovery

The immediate resolution was to ensure the "stuck" transaction to be included into a block by a Chia farmer.
A Chia farmer of reasonable size was asked to do this and farmed such a block.

To prevent such a situation in the future, a new block-fill algorithm was created that attempts to strike a balance, filling blocks efficiently, while also being careful not to take too much time in selecting transactions.
