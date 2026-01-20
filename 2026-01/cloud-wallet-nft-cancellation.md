# ISSUE NAME
## Issue Summary

A cloud wallet user created an offer including an NFT. When later attempting to cancel the offer they were presented with the error `unauthorized. organization does not have required feature flags: OFFER_CANCEL_OFF_CHAIN`.
The offer shows in the cloud wallet and on Dex's as cancelled but the NFT included in the offer is not in the creators vault address (as would be expected after a successful cancellation).

## Timeline (all times PST) DATE RANGE. All times approximations

1:06 PM January 14, 2026: User reported the issue using the cloud wallet in-app "Report an issue" form (CWB-139)

8:07 PM January 14, 2026: User inquired into the cloud wallet in-app reporting process and whether they should receive email follow-up

8:23 PM January 14, 2026: User reported the issue in discord

8:35 PM January 14, 2026: Users reported issue was relayed to keybase Cloud Wallet Support channel (internal channel)

8:54 PM January 14, 2026: The reported error was identified as being logged by our server 10:05 PM UTC

5:38 AM January 15, 2026: The user reported bug (from the in-app form CWB-139) was connected to the information relayed from discord

5:40 AM January 15, 2026: The bug ticket (CWB-139) was relayed to the Cloud Wallet dev manager group chat and noted as needing to be reviewed ASAP

5:46 AM January 15, 2026: The development ticket (CLD-998) was created to correspond with the bug ticket (CWB-139)

6:41 AM January 15, 2026: The support team updated the development ticket (CLD-998) with results of their onchain investigation into the NFT and offer file finding that the NFT was spent into its current XCH at time of offer creation.

6:06 AM January 15, 2026: The support team contacted the user in a private thread in discord to confirm their vault xch address

6:32 AM January 15, 2026: The development ticket was assigned to an engineer for further investigation

11:05 AM January 15, 2026: The user confirmed the vault address and indicated they reproduced the issue with a second NFT by following the same flow

11:32 AM January 15, 2026: The support team added a comment and image to the development ticket (CLD-998) indicating that a second NFT of the users was affected similarly with a different offer file that went through the same flow

4:31 AM January 20, 2026: The development ticket was reassigned to a different development team

8:11 AM January 20, 2026: The assigned development sprint was updated to include the development ticket in the next (not current) sprint


## Resolution and recovery

NA - we plan to fix this but the investigation is ongoing

## Additional Sections as required

NA

## Q&A

NA
