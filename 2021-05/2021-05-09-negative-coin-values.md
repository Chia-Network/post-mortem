## Issue summary

On May 9th 2021 at 6:48AM PDT, the blockchain stopped creating blocks at height 255108 for approximately 45 minutes, and then advanced very slowly for the next 3 hours until the hotfix was released.

We were not checking for negative values in the uint64 constructor, but instead were checking them when serializing. Therefore coins with negative values were added to the mempool. These blocks passed validation, but they did not get added into the blockchain due to negative values not serializing in uint64.stream. Farmers making these blocks would make blocks that did not make it into, or advance, the chain causing most farmers to be unable to create valid transaction blocks. Fortunately, no invalid blocks were added to the blockchain.

The fix added a check in the mempool and block validation, and did not disconnect peers who sent these invalid blocks (any peer 1.1.4 or older), making this update not mandatory. The blockchain advanced slower than normal for a day, while people updated their machines, and the difficulty adjusted.

## Timeline (UTC) All Times Approximate

- 12:48 (6:48 PDT): The last transaction block before the incident 255105, was farmed. Shortly after, blocks up to 255108 were farmed.
- 13:11: I (Mariano Sorgente) looked at my local node and realized the status was “Not synced”. This means there have been no transaction blocks in more than 7 minutes. I checked the public keybase chats and saw that many people have detected the same issue: their height was stuck at 255108. 
- 13:12: I notified the chia engineers chat that the blockchain was stuck and not moving forward. Almog and Arvid joined to help debug. We started debugging and noticed that there were many compact proof of time messages. I created a `no_compact` branch which commented out the handling of these optional messages.
- 13:26: Notified some medium size farmers to start running the no_compact branch. 
- 13:32: Made a post in announcements telling people to update to the no_compact branch if they are running from git. Continued debugging.
- 13:43: The blockchain started moving forward slowly, up to height 255111.
- 13:45: We noticed in the logs of block creation: WARNING  Consensus error validating block: int too large to convert
- 14:02: After debugging and adding logs, we noticed that there was an invalid coin amount in a block: 2021-05-09T23:02:01.463 full_node chia.full_node.coin_store: ERROR    Coin amount: -8551827461000000
- 14:19: The 1.1.4-hotfix branch was created, with a change to not accept valid blocks. I modified the public announcement to tell people to download this branch. The blockchain started moving slowly and with large gaps between blocks. 
- 14:20-2:00 Through the next two hours, we checked that there were no negative coins added to the blockchain, added a patch to reject these from the mempool, and added a patch to not disconnect old peers who sent us bad transactions. We also confirmed that the new code synced up successfully (no consensus change).
- 17:18: The final release was published (including mac and windows installers), and an announcement was made to notify users to upgrade.

## Root Cause

There is a uint64 class in the chia-blockchain python code which should only represent numbers from 0 to 2^64 - 1. However, the constructor did not throw when passing in a negative value. The validation happened at a later point, when serializing. This means it was possible to create a coin with a negative value. Furthermore, both the mempool validation and the block validation did not check for negative coins. This led to a few things happening: 
A transaction with a negative addition coin was added to the mempool and propagated to all nodes.
Farmers creating blocks added this transaction into the block, and these unfinished blocks validated successfully
When the VDF were added to these blocks to create the finished block, the finished block was not able to be added to the blockchain, because when trying to serialize the negative coin value, an exception would be thrown.
The reason why no invalid blocks were added to the chain, is that when we serialize the coins for inclusion of the DB, we called uint64.stream, which failed for negative values. Therefore the state transition failed, and the node reverted back to the last valid block, 255108. The validation was happening, but it was happening after propagating transactions and blocks, instead of before. 

The reason this caused issues with the blockchain moving, is that all farmers would try to include this negative transaction into their blocks, and this transaction did not get cleared from the mempool.

There is a fallback to creating an empty block, if a farmer realizes that they made a block with invalid transactions, to prevent these types of issues. However, since the validation of the block did not check for negative coin values, the farmers did not realize that their blocks were invalid, and broadcasted them with the negative coin.


## Resolution and Recovery

The hotfix we released is here: https://github.com/Chia-Network/chia-blockchain/commit/70c7229f0bf735a0ebb5087eb3a232288d9a7cf6

The first idea of this being an issue with compact VDFs was not correct, so we quickly started to look elsewhere. When we found out about the negative coin, we added a check in block_body_validation for any negative coin additions. We then added a check in the mempool to reject any transactions with negative coin additions, as well as a new error, COIN_AMOUNT_NEGATIVE. Finally, we caught the error in respond_unfinished_block, so that we would not disconnect 1.1.4 or older peers that had not yet updated. This was important, since we didn’t want to create a fork in the blockchain or divide the network into parts. The longer term fix is to add checks for negative values in uint64 construction.

After the hotfix was released, the blockchain moved forward more consistently, but still slowly. There were still a lot of people that had not updated, so the effective netspace was much lower than it should be, for that difficulty. The chain difficulty dropped from 215 to 167 due to the time in the stall, and due to people taking time to upgrade. The netspace dropped from 3.2EiB to around 2.6EiB (estimated). After the difficulty adjustment, blocks came much quicker than normal, due to many people upgrading. This led to more frequent forks, as well as double signage points. This is not an issue, but a longer stall could have potentially caused a larger difficulty adjustment, up to the max adjustment of a factor of 3. The difficulty then adjusted back to 209. 


## Lessons Learned and Future Improvements
Add more thorough testing and have strict testing standards for PRs into chia-blockchain. In particular, have negative tests in all overflow directions for all values.
Minimize the number of different types of data structures used.
Bug bounty in order to get more visibility on key parts of the code.
More automated alarms for when issues arise.
Have an incident response plan to notify the most experienced people, and potentially farmers as well.
Add an independent status page that e.g. tracks last transaction block time and alerts to a chia-status twitter account. Fix and improve our alerts for no new blocks being generated.
Add a notification in the GUI when a new version is available.

## Netspace after the incident
    
https://www.chiaexplorer.com/charts/netspace
