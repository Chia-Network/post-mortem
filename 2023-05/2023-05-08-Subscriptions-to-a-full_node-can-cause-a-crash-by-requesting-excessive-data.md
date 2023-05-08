# Coin and or PuzzleHash subscriptions to a full_node can cause it to crash by requesting excessive data.

## Issue Summary

The wallet protocol has a mechanism in which a wallet subscribes to updates to certain coin_id's or puzzle_hashes from a full_node.

The light wallet protocol allows a remote wallet to make such requests to a full_node.

The amount of information a subscription returns to the wallet is capped, but before this threshold is checked the data is pulled from the full_node database and can cause a full_node crash if the data amount is excessive.

## Timeline (all times PST)  10/31/2022 - 02/15/2023. All times approximations

10/31/2022 - The issue was discovered by Chia.

02/15/2023 - Version 1.7.0 of the Chia blockchain was released that limits how many coin_id's and / or puzzle_hashes can be queried from a full_node via a subscription.

## Resolution and recovery

This issue was addressed in 1.7.0 by: Limiting how many coin_id's and / or puzzle_hashes can be queried from a full_node via a subscription.

The thresholds are different when communicating with a local full_node or a remote full_node.

