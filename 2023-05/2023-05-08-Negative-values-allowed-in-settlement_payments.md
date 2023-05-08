# Negative values allowed in settlement_payments.clvm

## Issue Summary

https://github.com/Chia-Network/post-mortem/blob/main/2023-01/2023-01-31-Negative-values-allowed-in-settlement_payments.md
disclosed how the settlement_payments.clvm allowed for negative amounts and in the case of a Chia Asset Token creation, a special negative value of -113 (https://chialisp.com/cats) will run the TAIL of the CAT.

Version 1.6.1 of the Chia blockchain contains fixes to the code to prevent negative values, but due to a typographical error the old settlement_payments.clvm with the security issue
was still referenced in that code rendering the fix void.

## Timeline (all times PST)  01/31/2023 - 02/15/2023. All times approximations

01/31/2023 - A researcher submitted a bug report via bugcrowd (https://bugcrowd.com/chia-network-bb) detailing the security issue was not fixed in v1.6.1

02/15/2023 - Version 1.7.0 of the Chia blockchain was released with fixes for the Negative values allowed in settlement_payments.clvm issue.

## Resolution and recovery

This issue was addressed in 1.7.0 by: Asserting the value to be positive before creating the outputs in [https://github.com/Chia-Network/chia-blockchain/blob/main/chia/wallet/puzzles/settlement_payments.clsp](https://github.com/Chia-Network/chia-blockchain/blob/main/chia/wallet/puzzles/settlement_payments.clsp)
and referencing this updated ChiaList puzzle in the code.

The issue was not exploited on mainnet.

