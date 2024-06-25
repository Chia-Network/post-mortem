# Blockchain mainnet stall.

## Issue Summary

The timelord logic has a routine to detect if the timelord has a heavier unfinished block than the new peak provided by the full_node the timelord is connected to.
This would indicate that the timelord is working on a different chain that will be heavier and therefore can skip processing this provided new peak.

A bug in this code caused a chain stall when the full_node and timelord would get into a state where the full_node would provide a valid new peak and the timelord would skip it due to an edge case state.

The height that the chain stalled at was 4779033 block height with approximately a 10 minutes gap between blocks during the chain stall.


## Timeline (all times PST) 01/16/2024 - 02/28/2024. All times approximations

01-10-2024

7:32 PM - The chain stalled at height 4779033.

7:38 PM - Monitoring of the Chia blockchain detected the chain stall; no new blocks were created for a time period over 5 minutes. Automation restarted the timelords that Chia Network Inc. operates and this resolved the issue after the restart completed and new blocks were created by the farmers. Anyone that possesses a timelord could have also restarted the chain by restarting their timelord as well.

7:42 PM - New blocks being created again with approximately a 10 minutes gap between blocks during the chain stall.


01-31-2024 - The fix was added to the code base for version 2.2.0.

02-28-2024 - Version 2.2.0 was released that included the fix.


## Resolution and recovery

Chia Network Inc. was the only entity running the ASIC timelords at this time and those timelords were restarted via automation when the chain stall was detected.
This released the timelords and their connected full_nodes from the edge-case state they were in and new blocks were created shortly afterwards. However, had we not been monitoring, any running timelord that was restarted would have advanced the chain and the next new block would cause all other timelords to resume. 

The bug in the timelord code was fixed and introduced via the 2.2.0 release.

The commit fixing the issue can be found here:
https://github.com/Chia-Network/chia-blockchain/commit/221dab4f8a38199d38eb62041aae927a6c84341f

