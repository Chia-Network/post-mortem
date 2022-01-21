# Suspected Chia Employee Compromise

## Issue Summary

__Type of Incident Detected__: Potential compromise of employee's Google Account

__Initial Concerns__: Unexpected activity and login from the United Kingdom was noticed on employee's Gmail account page.

## Timeline (all times UTC )

Date/Time Stamps (Captured from keybase messages)

| Day | Date (2021) | Time (UTC) | Event |
|-----|-------------|-----------|-------|
| Wed | Dec 15 | 22:40 | Employee recognizes abnormal activity logged in his Gmail account. Employee reports this to another employee, who contacts the IPS team.
| Wed | Dec 15 | 22:45 | IPS team disables users Okta accounts and stars searching audit logs from google account for abnormal activity immediately.
| Wed | Dec 15 | 22:54 | It is noted in that the affected employee is experiencing issues with Keybase.
| Wed | Dec 15 | 23:02 | The IPS team discovered that the employee was using Express VPN, which provided an IP address in the UK, even though the user was connected to the VPN in Los Angeles.
| Wed | Dec 15 | 23:04 | The Sr. Director of IPS instructs the user to remove Express VPN, install tailscale, and remove third-party applications from his Google account.
| Wed | Dec 15 | 23:05 | No abnormal activity noted in the audit logs
| Wed | Dec 15 | 23:08 | IPS enables employee's Okta account.
| Wed | Dec 15 | 23:29 | Employee deletes third-party apps from Google account. 
| Wed | Dec 15 | 23:45 | Employee finishes logging into the Tailscale VPN and





## Root Cause

ExpressVPN routed the user's traffic with public-facing IP that originated with a location in the UK.


## Resolution and recovery

The IPS members responded quickly to identify that the VPN was the source of the issue.
## Corrective and Preventative Measure

Enforce a standard of using Tailscale company-wide. The employee did a fantastic job of observing something that looked suspicious and promptly alerting the appropriate personnel.

Continuous training of all Chia employee's will help to strengthen the company's security posture.
