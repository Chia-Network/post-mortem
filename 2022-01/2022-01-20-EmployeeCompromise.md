# Suspected Chia Employee Compromise

## Issue Summary

__Type of Incident Detected__: Potential compromise of employee's Google Admin Account

__Initial Concerns__: On Thursday, January 20th, 2021 at 22:04 (UTC) an employee's account was suspended by an admin account, per the audit logs. 
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
| Thur | Jan 20 | 22:06 | IPS team observed that the affected employee's Google Workspace account was suspended by another employee with administrator rights, when reviewing the audit logs in Google Admin. No abnormal login activity for detected for either employee.
| Thur | Jan 20 | 22:08 | The IPS team suspended the Okta account of admin employee.
| Thur | Jan 20 | 22:09 | The admin employee's Google Workspace account was then disabled. An IP address associated with the account suspension activity was determined to have a source from an Amazon Web Services (AWS) EC2 instance.
| Thur | Jan 20 | 22:19 | Communications were initiated with Google to take a deeper look into the issue.
| Thur | Jan 20 | 22:23 | The IPS team coordinated with AWS technical support chat to identify any information related to the AWS IP address.
| Thur | Jan 20 | 22:40 | Aside from the 'cookies reset and forced re-login' and 'Account suspended' actions in the audit logs, no other abnormal activity was present.
| Thur | Jan 20 | 23:15 | A zoom call with the AWS Incident Response Support team was started. The AWS team suggested verifying no abnormal activity exists in CloudTrail for Chia Network's two AWS accounts. They made the suggestion to create a SEV2 ticket and send an email to abuse@amazonaws.com for further troubleshooting. 
| Thur | Jan 20 | 23:17 | No abnormal activity was found in CloudTrail.
| Thur | Jan 20 | 23:20 | An AWS SEV2 support ticket was created.
| Thur | Jan 20 | 23:22 | An AWS Abuse case was created via email request.
| Thur | Jan 20 | 23:55 | The admin employee joined a zoom meeting with IPS team, to discuss the current situation and details.
| Fri | Jan 21 | 00:20 | Google Admin support chat was initiated for further investigation.
| Fri | Jan 21 | 01:31 | Cleared all active sessions and third party integrations from admin users' Google account. The admin employee was removed from superadmin access in Google.
| Fri | Jan 21 | 01:34 | Enabled admin employee's Google account only, leaving his Okta account disabled, allowing for the collection of email and access to calendar.
| Fri | Jan 21 | 16:49 | The admin employee's Okta account was enabled.
| Fri | Jan 21 | 17:02 | The affected employee's Google account was enabled. 
| Fri | Jan 21 | 18:20 | The admin employee mentioned that the cause of the account suspension may have been due to a Rippling integration.
| Fri | Jan 21 | 23:01 | IPS reached out to Rippling to discuss situation. They confirmed that the affected employee's account was suspended by Rippling, due to account information being changed and the application being integrated with the admin employee's Google Workspace account. 

## Root Cause

The affected employee was converted from a 1099 employee to a W-2 employee in Rippling. Due to the current application integration of Rippling into the admin employee's Google Workspace account.

## Resolution and recovery

Immediate communication was established when the affected employee noticed that his account was disabled. The IPS team took prompt action to contain the situation and seek a root cause of the incident.

## Corrective and Preventative Measure

We are continuing to improve and refine our response process for handling security incidents. 
