# ISSUE NAME
## Issue Summary

User that deleting a cloud wallet account also deletes the associated vault and since we do not have the ability to import vaults from the recovery key this leads to users loosing access to their funds.

## Timeline (all times PST) DATE RANGE. All times approximations

12:07 AM December 4, 2024: User reported in Discord that the "Delete Account" warning should be more "Dire". Note this was reported during the Cloud Wallet Open Beta.

12:17 AM December 4, 2024: Our team confirmed the message to which the user was referring as the popup indicating that the action is not reversible and that deleting permanently removes the users data and contents.

8:51 AM December 4, 2024: The user confirmed the message and reiterated that by deleting the account they have lost access to their vault's funds and there is no method of regaining access

10:02 AM December 4, 2024: An executive offered to send the user TXCH to make them whole again

8:29 AM February 26, 2025: The product team created documentation for the "Recovery Existing Vault" feature which would enable users to recovery their vault in the above circumstances

2:00 AM May 29, 2025: The product team created a development task to disallow users from deleting their accounts when funds are present in the users vault (CHIA-2977)

3:27 PM June 25, 2025: One of our developers came across the issue (unable to recovery a vault after deleting an account as the feature is not available) and submitted a development ticket CHIA-3241 later changed to CLD-653

2:18 PM August 15, 2025: Our support team raised 3 user facing issues as urgent including the issue that users cannot recover/import their vault to another account (in an internal keybase group chat)

11:30 AM September 19, 2025: Our support team relayed to our product team (in an internal keybase group chat) the need for users to be able to import/recover their vaults to another account

10:52 AM December 3, 2025: A different user reported the same issue (unable to recovery a vault after deleting an account as the feature is not available)

10:54 AM December 3, 2025: One of our Discord moderators confirmed to the user that they have lost access to their funds until we add the ability to recovery/import a vault in another account

10:54 AM December 3, 2025: Our support team relayed to the product team (in an internal keybase group chat) our need for users to be able to import/recovery their vault from a different account

11:15 AM December 3, 2025: This second user reported the same issue via the in-app reporting form (CWB-107)

3:50 AM December 20, 2025: A third user asked about what happens to their funds if they delete their cloud wallet account

6:54 AM December 20, 2025: Our support team confirmed what others in the community relayed to the user that they would lose access to their funds as currently we do not have the ability to recover/import vaults from other accounts

12:46 PM December 27, 2025: A fourth user inquired as to whether it is possible to restore a vault after deleting their account

12:47 PM December 27, 2025: One of our Discord moderators confirmed to the user that they would lose access to their funds until we add the ability to recovery/import a vault in another account

1:12 pm December 27, 2025: One of our execs reminded the users that sage will add this feature and others can too as the information "pieces" needed are all open source (indicating that vaults can be recovered onchain even if we have not added the feature)




## Resolution and recovery

NA - this issue is still present, we are prioritising the completion of instant recovery (rekeying via the spend key) so as to not introduce a different security issue (others being able to recover your vault when you are unable to instant rekey out the recovery key)

## Additional Sections as required

NA

## Q&A

NA
