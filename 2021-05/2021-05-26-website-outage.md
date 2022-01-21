
# Website Outage 5-26-21

## Issue Summary

In an effort to modernize the Chia branch naming structures the default branch for the chia network website, chia.net was migrated from master, to main.
When this type of change is made, the github sites background ci processes are supposed to engage and redeploy the site from the new renamed branch. Due to a bug in the ci system this did not occur.
The under lying ci system, appears to have encountered a fatal error.
Chia engineers attempted to rerun this ci process and these successive attempts to revive the site were also blocked by the ci pipeline bug.  
The github support team were able to reengage the stalled process with their administrative tools.
Github engineers are continuing investigation for a root cause to this issue. At this time they have advised that it is unlikely to occur again, but if so are on standby to assist with recovery.

## Timeline (all times approximate)
- 04:49 PM UTC Site repo branch Master, was renamed to Main
- 04:50 PM UTC Site repo pages configuration is modified to reflect new default branch name
- 04:52 PM UTC Site is hard down, chia operations engineers respond to alarms and being trouble shooting.
- 04:53 PM UTC Operations engineers revert site repo changes
- 04:56 PM UTC Operations engineers attempted to restart the stalled ci with several small commits to the site repo
- 05:00 PM UTC Support ticket was opened with Github enterprise support team
- 05:10 PM UTC Github support responds to ticket, begins investigation
- 05:15 PM UTC Stalled CI job is identified
- 05:30 PM UTC Further issues with the site are discovered, CI appears to have only run a portion of the site Setup
- 05:45 PM UTC Github engineers have fully recover stalled and bugged CI pipelines, site begins to recover fully.
- 06:00 PM UTC Site is fully recovered and Github support engineers begin investigation into the source of the ci bug.

## Root Cause

 A bug in the Github CI process responsible for updating github pages sites, stalled for an as of yet unknown reason. The change that enaged this CI job was a renaming of the Master default branch, to Main.
 Github support engineers continue investigation into the exact source of the stalled ci processes.

## Resolution and recovery

Github support engineers were able to recover the full functionality of github pages CI, and return the site to its previous working state. This was accomplished by manually reengaging the underlying CI process after clearing out the previous stalled tasks.

## Corrective and Preventative Measures

Going forward the Chia team is going to rebuild the site deployment process to allow for a more mature and modern release cycle.
The ideal pattern would be one with quick failover from a fatal delivery error, and some quality of life improvements to make content and other changes simpler, lowering their blast radius.
Most importantly Chia is aiming at a "five nines" solution for our website, as stability in our web presence is valuable to us and the goals of the chia project.
Chia operations engineers are working to build these patterns at this moment.
