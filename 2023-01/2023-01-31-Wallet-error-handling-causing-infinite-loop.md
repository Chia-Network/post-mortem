# Wallet error handling causing infinite loop

## Issue Summary

The Chia reference wallet keeps track of coins it can interact with by subscribing to them at a full node. The full node will communicate coinstate changes to the wallet if the subscribed coins state changes when a transaction block is processed.

The wallet keeps the data around the tracked coins in its own database, which will synchronize along with the blockchain database.

The wallet, on a coin spend, will run code that is applicable for the type of coin, for example a Chia Asset Token (https://chialisp.com/cats) or a Pool NFT Singleton (https://chialisp.com/pooling)
 
The way exceptions were handled in the wallet code could result in a bad coinstate causing the wallet to rewind its database a few blocks and retry to synchronize, encountering the same issue again when the same bad coinstate was processed. This would prevent the wallet from progressing past the block with the bad coinstate.

## Timeline (all times PST) DATE RANGE. All times approximations

09/07/2022 - A researcher provided details of the Wallet error handling causing infinite loop  issue via Keybase and Discord.

09/27/2022 - The researcher submitted a bug report via bugcrowd (https://bugcrowd.com/chia-network-bb) detailing the Wallet error handling causing infinite loop issue.

11/03/202 - Version 1.6.1 of the Chia blockchain was released with fixes for the Wallet error handling causing infinite loop issue.

## Resolution and recovery

This was issue was addressed in 1.6.1 by:
Adding try/catch sections around individual coinstate handling in the reference wallet code.

The issue was reported on mainnet in connection with DID validation of NFTs.

