# Negative values allowed in settlement_payments.clvm

## Issue Summary

Offers are a way to enable peer-to-peer asset exchange on the Chia blockchain (https://chialisp.com/offers/).

At the core of the offers functionality is the settlement payments puzzle, that spends the inputs from one party and recreates them as outputs for the counterparty. This action is applied to both sides of the offer at the same time.

The input spends are guarded via PUZZLE ANNOUNCEMENTS, only if the party inputs are spent and their puzzle announcements are asserted will the counterparty inputs spends succeed and vice versa.

The puzzle announcements enforce that the correct coin and value are part of the inputs of either side of the offer.

The code that creates outputs failed to check against negative values. In the case of a Chia Asset Token creation, a special negative value of -113 (https://chialisp.com/cats) will run the TAIL of the CAT.

When the TAIL execution outputs puzzle announcements that guard the input spends of the counterparty, then the requirement of the puzzle announcement is fulfilled and counterparty’s side of the offer trade succeeds regardless if the party’s inputs match the offer.

This allows for bypassing the explicit promise that in return of your input of coin(s) and their value you will get a specific type of coin(s) and value back when a counterparty takes up your offer.

## Timeline (all times PST) DATE RANGE. All times approximations

09/21/2022 - A researcher submitted a bug report via bugcrowd (https://bugcrowd.com/chia-network-bb) detailing the Negative values allowed in settlement_payments.clvm issue.

11/03/202 - Version 1.6.1 of the Chia blockchain was released with fixes for the Negative values allowed in settlement_payments.clvm issue.

## Resolution and recovery

This was issue was addressed in 1.6.1 by:
Asserting the value to be positive before creating the outputs in  https://github.com/Chia-Network/chia-blockchain/blob/main/chia/wallet/puzzles/settlement_payments.clvm 


The issue was not exploited on mainnet.
