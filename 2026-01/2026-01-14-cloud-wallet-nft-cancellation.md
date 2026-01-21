# Cloud Wallet NFT Cancellation Bug
## Issue Summary

A cloud wallet user created an offer including an NFT. When later attempting to cancel the offer they were presented with the error `unauthorized. organization does not have required feature flags: OFFER_CANCEL_OFF_CHAIN`.
The offer shows in the cloud wallet and on Dex's as cancelled but the NFT included in the offer is not in the creators vault address (as would be expected after a successful cancellation).

## Timeline (all times PST) DATE RANGE. All times approximations

2:06 PM January 14, 2026: User reported the issue using the cloud wallet in-app "Report an issue" form (CWB-139)

9:07 PM January 14, 2026: User inquired into the cloud wallet in-app reporting process and whether they should receive email follow-up

9:23 PM January 14, 2026: User reported the issue in discord

9:35 PM January 14, 2026: Users reported issue was relayed to keybase Cloud Wallet Support channel (internal channel)

9:54 PM January 14, 2026: The reported error was identified as being logged by our server 10:05 PM UTC

6:38 AM January 15, 2026: The user reported bug (from the in-app form CWB-139) was connected to the information relayed from discord

6:40 AM January 15, 2026: The bug ticket (CWB-139) was relayed to the Cloud Wallet dev manager group chat and noted as needing to be reviewed ASAP

6:46 AM January 15, 2026: The development ticket (CLD-998) was created to correspond with the bug ticket (CWB-139)

7:41 AM January 15, 2026: The support team updated the development ticket (CLD-998) with results of their onchain investigation into the NFT and offer file finding that the NFT was spent into its current XCH at time of offer creation.

7:06 AM January 15, 2026: The support team contacted the user in a private thread in discord to confirm their vault xch address

7:32 AM January 15, 2026: The development ticket was assigned to an engineer for further investigation

12:05 PM January 15, 2026: The user confirmed the vault address and indicated they reproduced the issue with a second NFT by following the same flow

12:32 PM January 15, 2026: The support team added a comment and image to the development ticket (CLD-998) indicating that a second NFT of the users was affected similarly with a different offer file that went through the same flow

5:31 AM January 20, 2026: The development ticket was reassigned to a different development team

9:11 AM January 20, 2026: The assigned development sprint was updated to include the development ticket in the next (not current) sprint

3:41 PM January 20, 2026: The resolution has been identified and requires three steps: 1. fixing the underlying issue that led to NFTs involved in offers using auto-split to not be properly tracked in the db making it so NFTs are no longer "lost" moving forward (pr2469), 2. Fixing the NFT signing logic to include the ability to spend any p2 puzzle (pr2466), 3. running a script on the various environments to ensure previously identified "lost" nfts are properly added to the DB

3:58 PM January 20, 2026: PR 2466 was merged and deployed to our dev environment

5:00 PM January 20, 2026: With a manual script performing the role of the change in PR 2469 the dev environment was tested and the changes associated with pr 2466 were confirmed as a resolution to the inability to spend the other P2 type

5:09 PM January 20, 2026: PR 2469 was merged and deployed to our dev environment, this was tested and confirmed as a resolution to the NFT being "lost" when included in an offer with auto-split

9:00 AM January 21, 2026: Our product team determined that these two PRs warrant a hot fix, version 0.2.53 was created as this hot fix version

12:26 PM January 21, 2026: 0.2.53 was deployed to our UAT testnet environment, this was tested and passed both for the fix and any potential regressions

1:13 PM January 21, 2026: 0.2.53 was deployed to our Prod testnet environment this was tested and passed both for the fix and any potential regressions

2:05 PM January 21, 2026: 0.2.53 was deployed to our Prod Mainnet environment and the script was run to resolve the known "lost" NFTs.

xx:xx PM January 21, 2026: The user was informed of the updates and confirmed that the NFTs are now visible and can be properly spent


## Resolution and recovery

Problem 1: NFTs involved in an offer using the autosplit feature are incorrectly moved to a reservation address for the offer
Resolution for previous instances: running a script on the various environments to ensure previously identified "lost" nfts are properly added to the DB
Resolution for future instances: fixing the underlying issue that led to NFTs involved in offers using auto-split to not be properly tracked in the db making it so NFTs are no longer "lost" moving forward (pr2469)

Problem 2: NFTs are hardcoded to be spendable only with the regular P2 coin spend
Resolution: Fixing the NFT signing logic to include the ability to spend any p2 puzzle (pr2466)


## Additional Sections as required

NA

## Q&A

NA
