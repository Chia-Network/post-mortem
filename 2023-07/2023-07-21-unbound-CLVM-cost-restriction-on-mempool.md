# unbound CLVM cost restriction on mempool

## Issue Summary

Transactions that are included in a block are aggregated from one or more spendbundles. Inside spendbundles coins are spent, with the reveal of the puzzle and their solution of the coins. The puzzles and solutions consist of CLVM code that is assigned a cost, based on bytes of the code, and usage of operands and conditions. Each node on the network runs the CLVM code with limitations on how much resources the execution can use up via restrictions on the CLVM cost.

The Chia default mempool code uses a CLVM cost restriction of 0.5 times the restriction that consensus uses (what is allowed in a block). The consensus constant is "MAX_BLOCK_COST_CLVM": 11000000000


Version 1.7.1 of the mempool doesn’t include verification code that the CLVM cost of a MempoolItem has to be <= `constants.MAX_BLOCK_COST_CLVM * 0.5`
This code was present in previous versions.

This verification code was removed because it was assumed that running the MempoolItem as a block generator would perform this check already, making the code redundant.

The chia_rs function called run_block_generator() contained a bug where if the byte cost exceeded the limit (5.5 billion in this case), the u64 wraps around and effectively removes the cost limit.

The removal of the verification in the mempool code and the chia_rs bug combined allowed for a MempoolItem to have an unbound CLVM cost when the byte cost exceeded the limit.

## Timeline (all times PST)   04/23/2023 - 05/03/2023. All times approximations

04/23/2023 - Mempool items with a cost exceeding `constants.MAX_BLOCK_COST_CLVM * 0.5` were reported by a community member in keybase.

05/03/2023 - Version 1.8.0 of the Chia blockchain was released with fixes for the mempool code and chia_rs.


## Resolution and recovery

This issue was addressed in 1.8.0 by: 

Reinstating the mempool verification code that the CLVM cost of a MempoolItem has to be <= `constants.MAX_BLOCK_COST_CLVM * 0.5`.

Fixing the cost calculation in chia_rs when the byte cost exceeds the limit.


The issue was present in mainnet and manifested itself by transactions being added to the mempool that exceeded the CLVM cost threshold. These transactions were not added to blocks as consensus checks on CLVM cost disallowed that.

Chia monitored the mempool for these excessive CLVM cost mempool items and found that they were limited in scope and barely exceeded the threshold limit, causing very limited impact on nodes on the network during the time that the mempool allowed them.


