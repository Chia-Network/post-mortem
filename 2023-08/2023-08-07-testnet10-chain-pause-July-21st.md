# testnet10 chain pause July 21st.

## Issue Summary

testnet10 is a blockchain based on the Chia codebase that is publicly accessible and supported by Chia. It’s used by Chia to test against both released code as well as code that is in development. 

Some code changes are present in the current codebase, but do not go into effect until a certain block height. This is the preferred method to implement soft forks / hard forks as it allows the user base uptake of the code change, in netspace percentage, to exceed 50%; a requirement for the fork to succeed.

On July 21st 2023 Chia deployed code on testnet10 nodes that Chia maintains to test CHIP-0013 (https://github.com/Chia-Network/chips/blob/0e3116e1a26554504230b2c3307a64c192928686/CHIPs/chip-0013.md) referenced as SOFT_FORK3 in the code, with a block height of 0 to when the change should go into effect.

The activation height of 0 caused all blocks (including historical blocks) to be validated against the new rules, making some blocks that were valid before (the CHIP-0013 rules did not exist when the block was created) now invalid.

Syncing to the blockchain became unstable as peers that communicated blocks now seen as invalid were banned. As a result updated nodes, including the timelords, were unable to sync the blockchain and new block creation stopped, resulting in a paused testnet10 chain.


## Timeline (all times PST) 07/21/2023 - 07/21/2023. All times approximations

07-21-2023 - Updated code was merged into Chia-Network/chia-blockchain with in effect height for SOFT_FORK3 of 0 and deployed, causing the testnet10 chain pause.

07-21-2023 - The code was reverted back to before the change and deployed, causing the testnet10 chain to resume.


## Resolution and recovery

The change to test SOFT_FORK3 was reverted from the codebase and the reverted code was deployed to testnet10 nodes that Chia maintains, allowing block production to continue per normal.



