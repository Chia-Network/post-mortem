# Mempool not updated at re-org.

## Issue Summary

Due to a bug in the Mempool code at the time of a re-org, if the latest block was a non transaction block, the mempool did not get updated until the next transaction block occurred.

When a farmer would have an outdated Mempool and add conflicting items into their farmed block, a block without any transactions would be created instead.

For more information on re-orgs and transaction blocks, visit:
https://docs.chia.net/consensus-foliage/


## Timeline (all times PST) 01/05/2024 - 02/28/2024. All times approximations

01-05-2024 - A bug report was created that informed Chia of the issue at https://github.com/Chia-Network/chia-blockchain/issues/17186

01-20-2024 - The codebase was updated with a fix at https://github.com/Chia-Network/chia-blockchain/pull/17370

02-28-2024 - Version 2.2.0 was released that included the fix. 


## Resolution and recovery

A fix was introduced to have the mempool update immediately with a reorg via Blockchain.get_tx_peak(), which returns the most recent transaction block (which may be the peak, if that is a transaction block).

For more information about the issue and resolution, visit:
https://github.com/Chia-Network/chia-blockchain/pull/17370
