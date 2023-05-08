# May 4th 2023 Discord API Abuse

## Issue Summary

In order to verify direct messages have been disabled when joining our Discord server, our bot sends them a message. If it fails to go through, it's disabled. When this scaled to the hundreds of users who joined the server, Discord flagged the bot as spam and revoked its API access.

## Timeline (all times approximate)

-   08:09 AM UTC Issue with accepting new users in the queue is identified by the Support team.
-   02:13 PM UTC Issue is looked into, identified as the bot being temporarily banned from Discord's API.
-   02:21 PM UTC Appeal is submitted to Discord, explaining our reliance on it for member verification.
-   03:34 PM UTC Action plan is created to temporarily restore member queue functionality. Manual verification is used as a temporary solution in the meantime.
-   04:59 PM UTC New bot is deployed with identical functionality except problematic API usage removed.
-   05:00 PM UTC Member queue is back to a functional state, everything picks up as normal.

## Root Cause

In order to verify that a user has disabled direct messages in the server's privacy settings, a bot has to attempt to send that user a message and check for an API permissions error. There is no way to directly check that user's privacy settings. The code was implemented this way, and worked fine on a small scale.

However, when the server launched and the initial flood of hundreds of users begun, subtle but noticable rate limiting issues begun to occur. They were considered to be normal since it was due to the influx of users, and ignored.

Even though users were instructed to disable direct messages, and sending them to each user was solely for verification purposes, the Discord API had no way to distinguish this from spam. The bot was flagged and restricted from using the API due to high usage of the direct message API endpoint.

## Resolution and Recovery

In order to temporarily solve this issue, we manually verified users who were not able to join while the bot was offline. At the same time, we worked to create a second bot account in order to unblock user verification.

The only difference is that the functionality for checking if direct messages are enabled was removed. This fix worked and doesn't run into ratelimiting issues like before, but does remove the validation.
