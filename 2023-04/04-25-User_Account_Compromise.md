# ISSUE NAME
Password compromise of an employees' Okta account.

## Timeline (PST) 4/21/23-4/25/23. All times approximations
_4/21/23_

11:12 AM: An email was received by the user from Okta about a login attempt from an unfamiliar IP address and location.

11:14 AM: User had forwarded the email of a successful sign-on attempt of his Okta account to IPS.

11:18 AM: The user logged in to his Okta account.

11:31 AM: IPS locked the Okta account for the user.

11:36 AM: A member of the IPS team reported a potential compromise to the rest of the company.

11:43 AM: Okta logs are reviewed by IPS.

11:55 AM: It was confirmed that no successful login occurred, due to failure of MFA by the attacker.

12:18 PM: Questioning of the user began to understand how the account may have been compromised.

12:26 PM: User reported that he had connected to wifi approx. 2 hours earlier. No other abnormalities or changes noted.

12:27 PM: The user reported clicking on a link from LinkedIn. The link was from a trusted individual which prompted for
           login access to microsoft. They did use okta to product the login credentials for microsoft. However, an
           invalid credentials response was received and the user is now reaching out to determine the legitimacy of
           the LinkedIn message.
1:31 PM: Course of action is to leave the affected laptop off until a factory restore of the macbook can be completed.
           The user will also ensure that passwords are changed for 1Password and Okta.
1:34 PM: It was determined that the source of the message on LinkedIn was not from the trusted individual, but
           from someone else using her account. We have determined the link to be malicious. LinkedIn was sent an email
           from the trusted individual that fraudulent activity had occurred on their account.
           1Password master password was reset.
2:57 PM: An IPS member re-enabled the user's Okta account and forced a password change.

_4/24/23_

~9:00 AM: The user's MacBook was erased and reset.

-4/25/23_

9:41 AM: SentinelOne installed and full disk scan was completed.

## Resolution and recovery

The root cause was determined to be a malicious that was sent to the user from a trusted LinkedIn user.
The trusted LinkedIn user was compromised, therefore, it came from a bad actor. Recovery included a reset of passwords
and a erase and reset of the user's MacBook. SentinelOne has been installed and a full disk scan completed.
