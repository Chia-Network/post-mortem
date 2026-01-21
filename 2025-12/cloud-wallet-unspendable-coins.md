# Cloud Wallet Unspendable Coins
## Issue Summary

Across three related Cloud Wallet bugs, there is a consistent pattern of:
- Coins or tokens being reported as spendable in one part of the UI but becoming unspendable or failing to spend in specific flows (especially combine).
- The full node, in some cases, rejecting transactions with WRONG_PUZZLE_HASH during validate_spend_bundle, indicating that constructed spend bundles do not match the underlying coin or puzzle state.

Specifically:
- CWB‑85: An NFT send fails when the node rejects the transaction with WRONG_PUZZLE_HASH. The user had previously sent the same NFT successfully.
- CWB‑101: Coins received in a wallet appear as spendable, but when attempting to combine them, the next step shows them as unspendable.
- CWB‑115: A subset of coins are included in total balance but not in spendable balance, and combine attempts on those coins consistently fail, even though related transactions appear as settled.

Taken together, these issues indicate a broader class of defects around:
- Correct construction of spend bundles (ensuring puzzle hashes align with on‑chain expectations).
- Accurate and consistent tracking of coin state (pending/locked vs spendable) across different wallet views and flows, particularly for combine and send operations.

## Timeline (all times PST) DATE RANGE. All times approximations

03:14 PM November 5, 2025 – CWB‑85 is created for an NFT send that fails when the full node rejects the transaction with a WRONG_PUZZLE_HASH error.

09:47 AM November 10, 2025 – Additional screenshots and context are attached to CWB‑85 to document the failing NFT send flow and error details.

09:27 PM November 21, 2025 – CWB‑101 is created to track a case where coins received into a wallet appear spendable, but when using the combine flow they are shown as unspendable.

08:39 AM December 4, 2025 – CWB‑85 and CWB‑101 are reviewed and moved into a selected‑for‑development state for further engineering work.

08:12 AM December 15, 2025 – CWB‑115 is created to track a separate case where some coins are counted in total balance but treated as unspendable, and combine operations on them do not succeed.

09:53 AM December 18, 2025 – Support documents how pending coins, offers, and unsigned transactions affect spendable balances and shares this guidance under CWB‑115.

08:08 AM December 23, 2025 – More detail is added to CWB‑115 about signed‑but‑not‑submitted transactions and coins created via coin management, and the issue is escalated for deeper engineering investigation.

09:30 AM January 6, 2026 – CWB‑115 is updated: general spendable tracking is improved, but combine still fails for a small subset of coins; this subset is flagged for focused analysis.

01:45 PM January 21, 2026 – Additional logging is enabled around coin state and transaction submission for this class of issues, and new combine attempts are requested to capture more detailed diagnostics.

02:50 PM January 21, 2026 – CWB‑115 is updated to confirm that combine attempts on the identified coins are still failing despite prior improvements.

03:19 PM January 21, 2026 – The problematic coins in CWB‑115 are correlated with internal coin identifiers for backend review and log correlation.

03:26 PM January 21, 2026 – Engineering confirms, via CWB‑115, that this is another instance of the node rejecting transactions during validate_spend_bundle with WRONG_PUZZLE_HASH, aligning the behavior with the earlier failure pattern seen in CWB‑85.

## Resolution and recovery

This issue persists but is being actively investigated, this post mortem will be updated as more information and resolutions are available

## Additional Sections as required

NA

## Q&A

NA
