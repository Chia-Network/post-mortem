# Oct 31 2021 Dust Storm

## Issue Summary

On Saturday Oct 30th 2021 an unknown actor began sending a large volume of transactions to the Chia mainnet blockchain, causing transactions to begin filling the mempool. As the mempool grew larger, several symptoms became apparent across the network with the most disruptive symptom being lower end nodes becoming clogged with the larger mempool and beginning to fall behind on the chain. Some peers with many slow connections fell behind the peak of the blockchain. Network wide transactions without a fee were also disrupted during these times of higher burst. Throughout the day on the 30th a few smaller sorties of transactions were sent. When the storm transactions were halted, the mempool cleared rather quickly at a rate of 20+ transactions per second, returning things to normal each time. It is important to note that during this incident and the following day’s, the network continued to make and sign blocks, and the chain never stopped.

Early in the morning on Sunday the 31st of October, a far larger storm of transactions began. The symptoms we initially noticed as worrisome on the 30th began to appear again as the mempool began to climb in size once more.(However at no point during this second storm of transactions did the mempool grow near the maximum defined size.) The largest impact to the network was once again that lower-power nodes fell off the network as they were not able to stay in sync as the mempool grew, which resulted in as much as ~5 EiB of farmer space being unable to farm. A secondary impact of transactions requiring fees to be processed in a timely manner also emerged, more on that later. Finally in some very rare cases we were seeing a higher than usual number of reorgs on the chain, which was likely due to some small network propagation issues as the network reorganized to take into account the loss of some of the weaker nodes. 

In addition to the two largest impacts, we saw many smaller symptoms that occurred in varying degrees depending on the quality of the majority of a given node’s peers. Block propagation slowed for some portion of the network due to slower nodes being too slow to catch up but not too slow to fall off the chain. For some other portion of the network, signage points were arriving late causing some missed proofs and partials, even very strong nodes were impacted if the majority of their connected peers were struggling. 

To expand on the transaction fees mentioned above, transactions have had fee support since launch, however the lower transaction volume compared to the high block space available resulted in the majority of transactions being sent without fees. Additionally, with the followup release of the pooling protocol there were UI gaps in PlotNFT transactions (while the PlotNFT fees were supported in the code, there were no actual UI elements to leverage them). Many 3rd parties were also unprepared to use fees due to them being unnecessary before now- therefore many 3rd party transactions (eg. pools and exchanges) were delayed as they were caught in the congestion. Before this point, it was unexpected that transaction volume of this density would pop up this early in the chain's life cycle, but it was encouraging that many 3rd parties in the ecosystem were easily able to catch up to this need and resolve the problem in their environment within the day. For the Chia team’s part, the latest release of 1.2.11 should correct all the outstanding issues related to fees, with the exception of plotNFT fees, which we will address in the 1.2.12 release. 


## Timeline (all times PST ) Saturday Oct 30th 2021 - Monday Nov 1st, 2021. All times approximations

Block based timeline 
Dust storm times (in block heights): 
1st wave: 1071900 - 1072525 
2nd wave: 1072850 - 1073850 
3rd wave: 1074650 - 1078400

12:45 PM PST Saturday Oct 30th: First wave of storm transactions begins

13:55 PM PST Saturday Oct 30th: Chia operations alarms focused on AWS timelord usage intended to let Chia Devops team know if there are larger issues, fire. Devops engineers began investigating. 

14:00 PM PST Saturday Oct 30th : Devops and Software Engineering teams establish this is likely not genuine traffic, load across the network seems nominal. Teams begin looking for more data and digging deeper into information about the block chain state.

14:17 PM PST Saturday Oct 30th: First wave of transactions slightly slows.

15:48 PM PST Saturday Oct 30th : first wave of transaction peaks at 1750 transactions in the mempool.

16:00 PM PST Saturday Oct 30th: Chia team tunes all alerting, establishes these transactions are either an unannounced test by a 3rd party, or something less friendly.

16:02 PM PST Saturday Oct 30th: Mempool is back to zero.

18:00 PM PST Saturday Oct 30th: Second wave of transactions begin. 

22:22 PM PST Saturday Oct 30th: Second wave peaks at ~2800 transactions. 

23:00 AM MST Saturday Oct 30th: Mempool is empty again.

03:30 AM PST Sunday Oct 31st: Final and largest wave of the dust storm begins. 

03:40 AM PST Sunday Oct 31st: Engineers online at this hour begin conversations about a rise in mempool again, which appears to be similar to the previous day's issues.

06:00 AM PST Sunday Oct 31st: Chia engineers begin work on potential improvements and optimizations discovered from analyzing the logs of instances reporting issues.

12:00 PM PST Sunday Oct 31st: Chia team decides that an optimization patch based on the learnings so far should be released as soon as possible. Plans begin to take shape for an emergency patch. The fixes for this patch are outlined and the work is assigned to senior engineers for discovery. 

13:30 PM PST Sunday Oct 31st: first peak ~5100 transactions in the mempool. 

16:05 PM PST Sunday Oct 31st: trough, roughly ~4250 transactions in the mempool.

02:54 AM PST Monday Nov 1st: second peak of ~5750 transactions in mempool. 

04:46 AM PST Monday Nov 1st: mempool is empty. 

10:00 AM PST Monday Nov 1st: Formal planning for the emergency patch is set, all engineering tasks are assigned for completion, with a target release date of 11/03. 

https://drive.google.com/file/d/1w7pQXcyztJOqev97wqJ-G2BvH7UJRukF/view?usp=sharing

## Root Cause

The ultimate root cause of all issues during the dust storm was slow block propagation. The increase in transaction volume directly slowed the propagation of blocks as nodes were busy processing transactions rather than blocks. Since validating transactions requires using a lot of disk I/O and CPU, some nodes were unable to both process large transactions volumes _and_ blocks in a timely manner. Before the 1.2.11 update, there was no max queue size for transactions, and there was no prioritization of blocks, so some very large block validation times, and propagation times were being seen. Furthermore, some inefficient locks were being held while validating transactions, which further slowed the speed of block propagation.

Since signage points are based on blocks, farmers cannot farm until they process the most recent block. This caused slow machines or nodes connected to many slow machines to miss many signage points and lose farming time.  

The full mempool also caused fee related issues, since the Chia client and pools were not properly set up to fully utilize fees. Before this incident, low transaction volume meant that zero fees were sufficient to get any transaction into the blockchain. During the dust storm, some pools were not able to claim pool rewards from the blockchain, and were also not able to make payouts to users, due to insufficient fees being applied to these transactions to prioritize ahead of the storm. Fortunately, when the storm ended and fee pressure returned to zero, the pools that had not implemented fees yet, were able to process the pending transactions and catch up. 

Another fee related issue was that Chia farmers were not able to switch pools during the storm, due to no UI elements to adjust the fee beyond the default of zero.


## Resolution and recovery

On Nov 1, 2021 at approximately 09:45 am UTC the unknown entity ceased their dust storm activities, shortly following that the block chain returned to normal state and existing fee pressure again returned to nominal. 

On November 4, 2021, 1.2.11 was released, which solved many of the issues that led to the symptoms experienced by farmers and users. The changes addressed both the improper handling of a high volume of transactions, and the impact of high TPS on consensus and farming. This included many optimisation fixes, the most important being:
Blocks are now always prioritized over transactions. No matter how many transactions a node receives, it will always validate and propagate blocks first. This also reduced competition for I/O.
A transaction queue was implemented, which streamlined and optimized how transactions were processed.
More parallelism and caching is used to validate transactions and signatures
Some locks which were causing the node to wait before propagating blocks were removed
Transaction dropping was implemented in cases where a node can not keep up with a very high number of transactions per second. This is not likely to ever happen on medium/high end machines and SSD storage (this is mostly targeted at RPi 4’s with slow storage and similar setups).
More efficient updating of mempool after adding blocks.

As a result of the Dust storm, we have improved our load testing capabilities and experimented on testnet with higher TPS than even those seen on the dust storm, and low powered machines were still able to keep up. However, there are still some issues remaining we would like to address, especially for computers with slow I/O and drives. Database optimization remains a priority to fix in the near future.

Chia has temporarily upgraded our min spec for low end nodes to still be the Pi4 4GB platform, but with the additional specification of an ssd for the block chain database and at least 1GB of swap space configured on the operating system. (In other words, we do not recommend having your database file on a local SD card.) We also do not recommend using the GUI on Pi4 4GB models. 

## Testnet Dust Storm
On Monday November 1st 2021, we began running our own version of a dust storm spam bot on testnet 7. Below is a graph of a similar duration of transactions being sent. Most critically you will note that our dust storm which we used to test the 1.2.11 changes pre release created as many as 65,000 transactions in the mempool which is 11.3 times more transactions in the mempool than the original dust storm attempt. This gives our engineers confidence that we have tested against similarly harsh conditions seen during the mainnet dust storm and beyond, and allows our team to trust that we have mitigated many of the performance issues present in the lower end of nodes with the 1.2.11 upgrade. 

https://drive.google.com/file/d/1A77Y8h2G-Naq272gG4aUxGXcmZVY7SlB/view?usp=sharing

## Q&A



__Q: Do you know who conducted the Dust Storm, or do you plan to find out?__

__A:__ Not formally no, and we have no plans to find out with certainty. While curiosity would be satisfied, determining the identity of the person serves us no purpose as we have no interest in taking any actions based on this information. While impactful and frustrating, ultimately all they did was simulate a real world load on the network similar to one we would have expected to see months from now, effectively running the same stress tests we run on testnet at a larger (and significantly more expensive) scale.


__Q: Why did you not test this kind of load on the network before now to know how it would react?__


__A:__ Essentially we did, however, like all things when testing, despite one’s best efforts no amount of simulations will replicate the real world experience of hundreds of thousands of nodes and farmers at scale in a real world environment. (Anyone who has played a popular video game online on launch day can attest to this experience!) That said, we have taken what we learned from the Dust Storm and modified our internal testnet load tests to throw a load significantly larger than anything seen during the Dust Storm at it when desired to create an even worse scenario than any we imagined (or have experienced) before.


__Q: Why were none of the optimisations and changes you implemented after the event already part of the code?__

__A:__ Like any software development project, we have always known there were areas that worked to expectations, but could be improved upon. You always have to balance optimizing old code along with the implementation of new things (like yesterday’s CATs launch), which means deciding between shipping something now and making it better later, not releasing it until it’s 100% baked but released much further down the road. If you have a solid picture of your needs and expectations (and use projections!) over the next year, you’re able to strategically release things incrementally and go back and optimize them over time before those optimizations are required. This then allows you to balance each release between new functionality and tuning up old functionality. Unfortunately, when something like this dust storm happens that basically shines a spotlight on those needed optimizations you’ve been sitting on (because they weren’t yet required), you now have to shift focus on pulling that work in closer. All this scenario did was bring our real-world usage projections forward by several months, so in a way it was an inconvenient bit of assistance in tuning up our network.


__Q: Why did you not already have fees?__
__A:__ There was a bit of a misconception going around that we launched without fees support, rest assured this is not true. The Chia blockchain has always supported fees, and infact in the first day or two people were mistakenly making transfers with some non-trivial fees applied. (Much to the joy of the farmers who farmed them!) However, what _was_ lacking, was the ability to set custom fees on some later-added components, such as PlotNFT commands in the GUI and CLI, as well as fee documentation in the Pooling reference code. On the former subject, some of this was an honest oversight due to lack of need raising awareness and some of it was a “we can adjust this a bit later long before it’s actually needed”. (See the above Q for more context). On the later issue of pools, we have always stressed from day 1 that our pooling protocol reference code was just that, _reference_ documentation to help provide initial guidance, and that we strongly advised any pools building PlotNFT support to only use the code as a starting point, and build their own solutions with technology and implementations they were comfortable and familiar with. Anyone who relied exclusively on the reference code was indeed without transaction fee support built in, but this is one of the many reasons why we explicitly warned against relying on this code in production, where it was never meant to run. Practicality dictates here was always going to be a world where fees were needed down the road, however we do not fault pools for thinking they were an “over the horizon” priority, much in the way we did.

__Q: Why did the netspace go down?__
__A:__ The netspace only fluctuated briefly, as a result of low power nodes going out of sync, which in turn made any farmers attached to them unable to farm, causing the available netspace to decrease. As netspace is estimated over a rolling time window, this meant that the space decreased to a slight delay behind the actual issues, and took an accordingly length of time to adjust back once it was over before normalizing once again.


