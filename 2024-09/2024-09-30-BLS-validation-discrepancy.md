# BLS validation discrepancy.

## Issue Summary

In chia-blockchain 2.0.0 we switched our BLS implementation to BLST.
BLST implements the IETF specification where Infinity G1 points are disallowed.
In our previous implementation (based on relic) we made an exception where we allowed infinity G1 points.
Our BLS cache, which caches pairings of pubkeys and messages from spends, to do less work when validating aggregate signatures on blocks, allows infinity G1 points.
BLST disallowing infinity G1 points, and our cache allowing infinity G1 points introduced a discrepancy in how blocks are validated in different circumstances.


## Timeline (all times PST) 06/20/2024 - 09/17/2024. All times approximations

06-20-2024 - Version 2.4.0 was released that included Soft fork 5: disallow infinity G1 points as public keys in AGG_SIG_\* conditions.

09-17-2024 - Soft fork 5 activated at block 5,940,000.


## Resolution and recovery

In the condition parser for AGG_SIG_\* conditions, the public key is parsed and validated. The fix was to also check if the public key was infinity.
Any AGG_SIG_\* condition whose public key is infinity, is ignored. As if the condition did not exist. This is safe because an infinity public key always produces an infinity signature (regardless of what’s being signed). An infinity signature (G2 point) is an identity operation when aggregating with another signature. I.e. The aggregate signature is not affected by these conditions.

The change to the condition parsing to ignore or reject infinity G1 points on AGG_SIG_\* conditions:
https://github.com/Chia-Network/chia_rs/pull/533

Soft fork 5 was introduced in version 2.4.0 updating the consensus rules to always disallow infinity G1 points on AGG_SIG_\* conditions with block 5,940,000 as activation.

