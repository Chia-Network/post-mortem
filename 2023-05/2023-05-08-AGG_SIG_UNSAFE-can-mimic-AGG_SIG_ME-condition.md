# AGG_SIG_UNSAFE can mimic AGG_SIG_ME condition

## Issue Summary

Signatures are used to sign spendbundles (transactions), such that the puzzle execution can assert the signature is correct via AGG_SIG_UNSAFE or AGG_SIG_ME conditions.

Both AGG_SIG_UNSAFE and AGG_SIG_ME conditions take a public_key and message, the difference is that AGG_SIG_ME adds the coin_id and additional_data (that is specific to the Chia blockchain) to the message that is signed.

Due to the message's ability to have any value (within size limits), it could mimic the AGG_SIG_ME condition where coin_id and additional_data are appended to the message when using AGG_SIG_UNSAFE.

The core problem was that a coin issuing an AGG_SIG_ME and including its signature in the aggregate could be replaced by a spend issuing an AGG_SIG_UNSAFE and including the attacked coin's coin ID, and including the "additional_data" used for AGG_SIG_ME, in the message it signed. 

The aggregate signature would be the same, even though the coin spend was replaced. 

The worst case scenario is joining two coins, only one of the spends creates the new output. 

An attacker could replace the spend creating the output with their own spend, and compensate for its signature by issuing an AGG_SIG_UNSAFE that mimics its AGG_SIG_ME, thus, gaining access to the coins for spending maliciously. 

The soft fork (that just took affect) limits AGG_SIG_UNSAFE in a way that makes it impossible to mimic an AGG_SIG_ME. they are now, effectively, in separate domains.

## Timeline (all times PST)  12/01/2022 - 05/07/2023. All times approximations

12/01/2022 - The issue was discovered by Chia.

02/15/2023 - Version 1.7.0 of the Chia blockchain was released that enforces AGG_SIG_UNSAFE messages are not allowed to end with the Chia blockchain specific additional_data at the soft fork block height of 3630000 by consensus rules. The mempool code enforces this restriction immediately.

05/07/2023 - The soft fork at block height 3630000 went into effect.

## Resolution and recovery

This issue was addressed in 1.7.0 by: Making AGG_SIG_UNSAFE condition invalid if the message ends with additional_data at soft fork height 3630000.


No AGG_SIG_UNSAFE messages ending with additional_data were found in the revealed puzzles / solutions on mainnet.

