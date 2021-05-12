
# 5-10-21 Wiki Attack

## Issue Summary

At aprproximately <Timestamp> on 5-9 The user Bitmexbot, edited the installer instruction page of the wiki. To point the install links to a malicious URL <URL HERE>

## Timeline (all times UTC) Monday May 10th, 2021

At approximately 09:15:04 UTC an edit was made to the install documents on the Github wiki, pointing the windows install link to (MALICIOUS LINK!!!! VIRUS) https://rvmis.com/vendor/ChiaSetup-1.1.4.rar this url is now defunct.

At approximately 09:30:00 UTC Reports of the change began to reach Chia tech support on keybase.

At approximately 10:00:00 UTC Chia operations personnel investigated the malicious edits, and began corrective actions along with internal discussion of the problem. Primarily locking the wiki, and combing for other malicious edits.

At approximately 10:03:52 UTC Mariano reverted the malicious change.

## Relevant Commit history
commit 9b3a733a99f0ae5de143c6616bfcd4c945151420
Author: Mariano Sorgente <3069354+mariano54@users.noreply.github.com>
Date:   Mon May 10 19:03:52 2021 +0900

    Updated INSTALL (markdown)

commit a3c398b44333a0e9278c1059eaf3582b5822bf90
Author: Chia-Network <53834631+Bitmexbot@users.noreply.github.com>
Date:   Mon May 10 11:15:04 2021 +0200

    Updated INSTALL (markdown)

## Root Cause
Maliciou users attempted to redirect downloads to (MALICIOUS LINK!!!! VIRUS) https://rvmis.com/vendor/ChiaSetup-1.1.4.rar

## Resolution and recovery
wiki is locked, users banned

## Corrective and Preventative Measures
did analysis found <Findings>
Wiki page wont have installs anymore
new install page here <URL>
