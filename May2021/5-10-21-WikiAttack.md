
# 5-9-21 Wiki Attack

## Issue Summary

At aprproximately <Timestamp> on May 9th, 2021, the user account *Bitmexbot*, edited the installer instruction page of the wiki to direct the link for the Windows installer to a third-party host unassociated with the Chia project. This host was likely compromised and used as a jump-point/payload distributor by an as-yet unknown group. The installer delivered a malware payload instead of the Chia installation files.

## Timeline
> (all times in Pacific Time) - I'd use UTC

## Root Cause
Maliciou users attempted to redirect downloads to a host unrelated to the Chai project. This host is likely a third-party host previously compromised and used to seed content and/or act as a jump point.

Efforts to analyze the malware are ongoing at this time. These efforts include both security professional analysis and submission to industry standard pattern analysis engines. The [VirusTotal information](https://www.virustotal.com/gui/file-analysis/Y2JmMzVhNTUyZDRlZDliZjZhMmMxODE5MGQwYmQ4NmI6MTYyMDgzMTAzOA==/detection) indicates that a number of commercial antimalware engines now detect this malware installer.

## Resolution and recovery
The wiki has been secured and the account used to make the modification has been banned.

## Corrective and Preventative Measures
Securing the wiki was the first corrective action. Further the wiki will no longer include a link to the installer binaries. These binaries can now be found at <link>

The team remains vigilant to activity on the Chia project resources at this time.

The new installation page can be found at <link>
