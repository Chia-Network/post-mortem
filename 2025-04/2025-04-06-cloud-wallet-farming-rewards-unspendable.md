# ISSUE NAME
## Issue Summary

Initially it was reported that farming rewards sent directly to the cloud wallet (using the vault address as the farming or pool rewards address) would not appear in the transaction list, with latter reports indicating that these funds were not spendable.

## Timeline (all times PST) DATE RANGE. All times approximations

4:33 PM April 6, 2025: A user reported that they set their vault address as their farming rewards address and while the funds appear in the vault balance the associated transactions are not appearing (while cloud wallet is in closed beta) (CHIA-2672)

2:37 AM April 28, 2025: Our development team fixed the issues surrounding the rewards not appearing in the transaction list (CHIA-2672)

5:51 AM July 21, 2025: Our development team noted an alert from the testnet environment indicating issues syncing farming rewards from non-tx blocks as there is no timestamp (CLD-504)

6:29 PM July 24, 2025: A second user reported that farming rewards are not appearing in their vault, this extends to both the transaction list and the balance itself (CHIA-3490)

6:01 AM July 25, 2025: Our support team confirmed the users issue with them and requested more information from the user

9:57 AM July 25, 2025: The third user provided the additional information regarding their vault and our support team relayed that to the product team to help in their investigation

12:09 PM August 27, 2025: A fourth user reported that farming rewards are not appearing in their transaction list NOR their vault balance making the funds unspendable (while cloud wallet is in Open beta)

12:15 PM August 27, 2025: Our support team confirmed with the fourth user that this is a known issue currently being investigated and a potential workaround is to force a resync of the wallet by sending funds to it

8:01 AM September 1, 2025: The second issue reporter indicated that now not only is their transaction list incorrect but their balance is also (suggesting a different cause than what was originally solved)

7:34 AM October 2, 2025: Our support team (per the request of our product team) contacted the three users (all but the original reporter as the Product team was in direct contact with them) for more information on their vaults and unspendable farming rewards issues

8:07 AM October 13, 2025: The second issue reporter indicated that the issues now extend to all xch and CATs being sent to the wallet (indicating a deeper issue). Our support team collected the information and consolidated it to development ticket CLD-530

5:42 AM November 10, 2025: A fix for syncing farming rewards from non-tx blocks was merged to be part of the November 12 release

10:00 AM November 27, 2025: On the weekly architecture call one of our tech leads mentioned an issue they have been seeing in development of `MAX_PARAMETERS_EXCEEDED` likely related to a recent update to the chia blockchain nodes (previously the related cloud wallet queries silently failed, now we have this error returned)

12:48 PM December 2, 2025: One of our developers relayed that they are seeing errors in their local development environment of `MAX_PARAMETERS_EXCEEDED`

6:40 PM December 2, 2025: One of our tech leads confirmed the MAX PARAMETERS error is known and he has a PR in place to resolve the issues in the instances he could find

8:18 AM December 8, 2025: The second issue reporter indicated that their issues persist and still affect all XCH and CAT tokens now (not only farming rewards)

8:40 AM December 8, 2025: Our support team identified an alert on the Cloud Wallet mainnet environment that coincides with the attempts to sync the second users vault (error indicates MAX_PARAMETERS_EXCEEDED while attempting to sync)

1:42 PM December 8, 2025: One of our developers inquired about a postgres upgrade with the goal of narrowing down the error `MAX_PARAMETERS_EXCEEDED` to specific queries

3:54 AM December 18, 2025: We deployed a fix to the mainnet cloud wallet instance intended to resolve the max parameters issues for vault balances

1:54 PM December 19, 2025: The second issue reporter has confirmed that their farming rewards, xch, and CAT balances plus transaction lists are now correct and spendable

## Resolution and recovery

Two main resolutions:
CLD-504: Non-tx block farming rewards error during sync due to no timestamp -> we updated the sync function to estimate the non-tx block timestamp
CLD-530: MAX PARAMETERS issue created an inability to sync the balance or transaction list (affected more than just farming rewards) -> we optimized the sync for balances and transaction lists to chunk requests

## Additional Sections as required

NA

## Q&A

NA
