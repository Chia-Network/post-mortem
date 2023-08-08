# testnet10 chain pause July 25th.

## Issue Summary

testnet10 is a blockchain based on the Chia codebase that is publicly accessible and supported by Chia. It’s used by Chia to test against both released code as well as code that is in development. 

Some code changes are present in the current codebase, but do not go into effect until a certain block height. This is the preferred method to implement soft forks / hard forks as it allows the user base uptake of the code change, in netspace percentage, to exceed 50%; a requirement for the fork to succeed.

On July 24th 2023 Chia deployed code on testnet10 nodes that Chia maintains to test CHIP-0013 (https://github.com/Chia-Network/chips/blob/0e3116e1a26554504230b2c3307a64c192928686/CHIPs/chip-0013.md) referenced as SOFT_FORK3 in the code, with a block height of 2,960,728 (July 25th) to when the change should go into effect.

Due to circumstances the timelord nodes that Chia maintains for testnet10 were not included in the deployment of the code change and were either running on the codebase with activation height 0 for SOFT_FORK3 and unable to sync to the testnet10 chain, or were running on the codebase of commit e325bf99da3336413da40ab46887f530e59d5968 that did not contain logic for CHIP-0013.

On July 25th, the first SOFT_FORK3 ineligible block occurred at a height of 2,960,959. Any farmer, node, etc. that was running the then-current SOFT_FORK3 code was stalled at this height because the heavier chain that was still moving was no longer compliant with their consensus rules, and there was no timelord to maintain the chain with SOFT_FORK3 accepted blocks. While nodes / farmers running the updated code were stalled, there were still at least one farmer and one timelord running code with no SOFT_FORK3 logic in it, creating the heavier chain.

At a height of 2,961,277, Chia deployed the codebase that activated SOFT_FORK3 at height 2,960,728 to the timelords maintained by Chia that were on commit
e325bf99da3336413da40ab46887f530e59d5968 (that had no SOFT_FORK3 logic) and were moving the chain forward that had no SOFT_FORK3 logic.

The timelord codebase update that set the activation height to a block height in the past, caused instability in the blockchain sync process due to blocks that were created, that were not compliant with SOFT_FORK3 rules, between heights 2,960,959 and 2,961,277

Problem 1:
All of the peers that were stalled at the earlier height of 2,960,959 were receiving valid weight proofs from the peers that were on 2,961,277, but as soon as they started validating individual blocks from those peers, validation would fail because of SOFT_FORK3 rules, and peers would get banned.

Problem 2:
The timelords that were previously running on commit e325bf99da3336413da40ab46887f530e59d5968, when updated to a SOFT_FORK3 compliant version, had blocks in their history that were not SOFT_FORK3 compliant after the in effect block height of 2,960,728 because the blocks were validated at a time when the  SOFT_FORK3 rules did not exist.

As a result this testnet10 chain block production stopped. At that time both testnet10 nodes that had an updated codebase for SOFT_FORK3 and those without were not producing blocks.


## Timeline (all times PST) 07/24/2023 - 07/25/2023. All times approximations

07-24-2023 - Updated code was merged into Chia-Network/chia-blockchain with in effect height for SOFT_FORK3 of 2,960,728 and deployed, except for the Chia maintained timelords.

07-25-2023 - SOFT_FORK3 at block height 2,960,728 went into effect, the testnet10 chain stalled for nodes that enforced SOFT_FORK3.

07-25-2023 - Timelords were updated with SOFT_FORK3 of 2,960,728, the testnet10 chain stalled for nodes that did not enforce SOFT_FORK3.
 
07-25-2023 - Updated code was merged into Chia-Network/chia-blockchain with in effect height for SOFT_FORK3 of 2,997,292 and deployed. The testnet10 chain recovered for nodes that never enforced SOFT_FORK3 and for those with the updated code.


## Resolution and recovery

Chia deployed code on testnet10 nodes on July 25th that Chia maintains to test CHIP-0013 (https://github.com/Chia-Network/chips/blob/0e3116e1a26554504230b2c3307a64c192928686/CHIPs/chip-0013.md) referenced as SOFT_FORK3 in the code, with a block height of 2,997,292 (August 3rd) to when the change should go into effect (https://github.com/Chia-Network/chia-blockchain/commit/3a81506dc7a513265afbae61e19bcfa16a013beb).

Chia ensured this code was deployed on all types of testnet10 nodes it maintains, including timelords. As a result the testnet10 nodes that were updated to the SOFT_FORK3 codebase with an in effect block height in the past, could now accept noncompliant SOFT_FORK3 blocks again as the in effect block height was now in the future.



