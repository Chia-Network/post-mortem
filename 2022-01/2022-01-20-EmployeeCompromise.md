# Suspected Chia Employee Compromise

## Issue Summary

__Type of Incident Detected__: Potential compromise of employee's Google Account

__Initial Concerns__: On Thursday, January 20th, 2021 at 22:04 (UTC) Employee's account was suspended by an admin account, per the audit logs. 
                      However, the admin did not perform the suspension of the account and the action originated from an IP address associated with an
                      AWS EC2 instance.

__Involved Parties__: 
Affected employee - Individual whose account was suspended.

Admin employee: Individual whose account performed the suspension action.

IPS team: Infrastructure, Platforms, and Security members who responded to the potential incident.

## Timeline (all times UTC )

Date/Time Stamps (Captured from keybase messages)

| Day | Date (2021) | Time (UTC) | Event |
|-----|-------------|-----------|-------|
| Thur | Jan 20 | 22:40 | Affected employee logged out of Google Workspace account and received a prompt that his account was disabled, upon attempting to log back in.
| Thur | Jan 20 | 22:06 | IPS team disables users Okta accounts and searched for abnormal activity. No abnormal login activity for affected employees.
| Thur | Jan 20 | 22:08 | The IPS team suspended the Okta account of admin employee.
| Thur | Jan 20 | 22:09 | The admin employee's Google Workspace account was then disabled. The IP address was determined to have a source from an AWS EC2 instance.
| Thur | Jan 20 | 22:19 | The IPS initiated communications with Google to take a deeper look into the issue.
| Thur | Jan 20 | 22:23 | The IPS team coordinated with AWS technical support chat to identify any information related to the AWS IP address.
| Thur | Jan 20 | 22:40 | Aside from the 'cookies reset and forced re-login' and 'Account suspended' actions, no other abnormal activity exists.
| Thur | Jan 20 | 23:15 | Zoom call with Incident Response Support AWS team. The AWS team suggested verifying no abnormal activity exists in CloudTrail for Chia’s two AWS accounts. They made the suggestion to create a SEV2 ticket and send an email to abuse@amazonaws.com for further troubleshooting. 
| Thur | Jan 20 | 23:17 | No abnormal activity was found in CloudTrail.
| Thur | Jan 20 | 23:20 | AWS SEV2 support was requested.
| Thur | Jan 20 | 23:22 | AWS Abuse case was created.
| Thur | Jan 20 | 23:55 | The admin employee joined a zoom meeting with IPS to discuss the current situation and details.
| Fri | Jan 21 | 00:20 | Google Admin support chat was initiated for further investigation.
| Fri | Jan 21 | 01:31 | Cleared all active sessions and third party integrations from admin users' Google account, enabling access to his calendar and email.
| Fri | Jan 21 | 01:34 | Enabled admin Google account only, leaving his Okta account disabled.
| Fri | Jan 21 | 16:49 | Admin employee's Okta account was enabled.
| Fri | Jan 21 | 17:02 | Affected employee was Google account was enabled. 
| Fri | Jan 21 | 18:20 | The admin employee mentioned that the cause of the account suspension may have been due to a Rippling integration. The account may have been automatically disabled when the affected employee's information was changed.
| Fri | Jan 21 | 23:01 | IPS reached out to Rippling to discuss situation. They confirmed that the affected employee's account was suspended by Rippling, due to account information being changed and the application being integrated with the admin employee's Google Workspace account. 

## Root Cause

The affected employee was converted from a 1099 employee to a W-2 employee in Rippling. Due to the current application integration of Rippling into 

## Resolution and recovery

Immediate communication was established when the affected employee noticed that his account was disabled. The IPS team took prompt action to contain the situation and seek a root cause of the incident.

## Corrective and Preventative Measure

We are continuing to improve and refine our response process for handling security incidents. 
