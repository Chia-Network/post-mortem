# GUI clickable links from NFT metadata.

## Issue Summary

A community member disclosed to Chia Network Inc. that at the time of an NFT’s minting, an attacker could add malicious links to the off-chain metadata. When a user later viewed the NFT in the reference wallet GUI, the links would be made clickable.
This would occur because the GUI would expect any links to correspond to the NFT’s metadata, license, etc. However, the links could instead point to any URI with potentially malicious content.

A few things to consider:
- The user would need to click a malicious link in order to activate the payload; no malicious activity was possible from simply viewing an NFT.
- Upon clicking a malicious link, most operating systems would display another dialog with a warning message that this link might be malicious. The user would then need to authorize the malicious link to be executed.
- We found no evidence that this attack was ever attempted, successfully or not.

We deemed that this potential exploit was sufficiently urgent to include a fix in the 2.3.0 release, which went out within a week of us learning about the issue.


## Timeline (all times PST) 04/26/2024 - 05/01/2024. All times approximations

04-26-2024 - The security issue was disclosed to our team.

04-29-2024 - The fix was added to the code base for version 2.3.0.

05-01-2024 - Version 2.3.0 was released, which included the fix.


## Resolution and recovery

Starting in version 2.3.0, the reference wallet no longer allows links from NFT metadata to be clickable. Any links that do exist will still be displayed (though not clickable), thereby giving the user the option to copy them and paste them into a browser window.

The source code for the resolution can be found in the following Pull Request:
https://github.com/Chia-Network/chia-blockchain-gui/pull/2331

