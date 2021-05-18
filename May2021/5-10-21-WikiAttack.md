## Issue Summary

At approximately 09:15:04 Monday May 10th (UTC), 2021, the user account *Bitmexbot*, edited the installer instruction page of the wiki to direct the link for the Windows installer to a third-party host unassociated with the Chia project.

## Timeline (all times UTC) Monday May 10th, 2021. All times approximations

- 09:15:04 UTC an edit was made to the install documents on the GitHub wiki, pointing the windows install link to (MALICIOUS LINK!!!! VIRUS) ```https://rvmis.com/vendor/ChiaSetup-1.1.4.rar``` (this url is now defunct).

- 09:30:00 UTC Reports of the change began to reach Chia tech support on keybase.

- 10:00:00 UTC Chia operations personnel investigated the malicious edits, and began corrective actions along with internal discussion of the problem. Primarily locking the wiki, and combing for other malicious edits.

- 10:03:52 UTC Mariano reverted the malicious change.

## Root Cause

Malicious user redirected downloads to (MALICIOUS LINK!!!! VIRUS) ```https://rvmis.com/vendor/ChiaSetup-1.1.4.rar``` with an unreviewed wiki edit, via what we believe to be the Github web UI.

## Relevant Commit history

commit 9b3a733a99f0ae5de143c6616bfcd4c945151420
Author: Mariano Sorgente <3069354+mariano54@users.noreply.github.com>
Date:   Mon May 10 19:03:52 2021 +0900

    Updated INSTALL (markdown)

commit a3c398b44333a0e9278c1059eaf3582b5822bf90
Author: Chia-Network <53834631+Bitmexbot@users.noreply.github.com>
Date:   Mon May 10 11:15:04 2021 +0200

    Updated INSTALL (markdown)


## Resolution and recovery

Mariano published a fix to the malicious URL immediately upon noticing the issue. - Total time to resolution approximately 49 minutes.

We immediately contacted GitHub support to get the malicious user banned. This action took about 48 hours but the user is now banned from GitHub.

We immediately locked the wiki and will no longer be sharing install links on publicly editable pages.

Security professionals continue analysis of the malicious .RAR file and have findings about its possible origin, intent, and potential impact on any victims.

## Malicious file findings - Preliminary

The RAR file contains two win32 executables, ChiaSetup-1.1.4.exe and mkvtool.exe along with several DLLs, TXT files, a font folder with fonts and a folder with object handlers for various file formats. The attackers appear to have deployed using the mkvtool to unpack an image and begin the installation of backdoors. Contents appear to include a network scanner and media rendering binaries (likely packaged with mkvtool). When the binaries execute, several Windows services are stopped (event logs, wmi, wer) and then installation to the user profile occurs (\AppData\Local\Temp). This directory contains a payload of C2 files:

    start.vbs
    get-content.ps1
    ready.ps1
    .ses (session token)

Network traffic analysis of the installation showed traffic utilizing the Windows Background Task Scheduler service and Background Host Transfer binary. IP addresses pointed to cloud service provider IP ranges (Azure and IBM Cloud). These addresses are likely no longer used by the attackers.

[VirusTotal details](https://www.virustotal.com/gui/file/476cdefcc0cd45525c7dc73a1dd0a1c97698c047dfaacdbf70ad32fc6bb65ee4/detection) a list of AV vendors catching this package.

https://drive.google.com/file/d/1KHx5zL7mUOf2QFt9mWxexhE4sxa_Zxc6/view?usp=sharing

https://drive.google.com/file/d/1qMWQK2n6LDKrmurfhTvhp0Q9X-Uzz6lS/view?usp=sharing

## Corrective and Preventative Measure

The installation wiki page will be changed to no longer include installer links. The installers will live at chia.net/install and download.chia.net. The wiki will remain locked until that time. 

