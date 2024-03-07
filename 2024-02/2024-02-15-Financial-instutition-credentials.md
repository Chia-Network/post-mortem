# Financial Institution unauthorized access incident

On Jan 29, 2024, a connection was established between the main financial admin tool used by Chia Network Inc (CNI) , and one of their major banking partners. A CNI  employee’s credentials were used to set up this integration. This integration worked as expected until February 14, 2024. Some time during the day on February 14, 2024 the IP address the integration ran from, was changed.

On February 15, 2024 the integration ran, and because of a quirk in the system used by the bank, because the IP the integration requests were coming from changed, the new IP used by the financial admin tool was reported as a distinct and possibly malicious use of the credentials attached to this integration.

This spawned a malicious activity report which reached CNI security and the employee at roughly the same time. 7:56 AM UTC.

At 1:15 PM UTC, The CNI employee reported the incident to the security team, who were already investigating the alert. At this time the user's access to CNI systems was disabled.

After some initial coordination around 3:50 PM UTC the financial institution was reached and in error, confirmed that the account had in fact been logged into, from the IP in question, and that MFA had been used on the user's account. Luckily the account in question was read only, and no funds had been tampered with. This is also when the internal incident response process was iniated. CNI team was awaiting contact with the fraud prevention team at the financial institution as well.  

In the interim, CNI security team did a cursory investigation on the IP reported by the financial institution and established the location of use, and owner, as well as the abuse contact.

Also during this period the CNI employee attempted to contact their MFA provider and gain information on how a malicious party might have been able to also use this factor.

At this point forward CNI security team and employees proceeded with the assumption of active compromise of the user’s workstation, and possibly home network. Because of the multiple federally regulated services involved, evidence gathering was initiated. The CNI employee was instructed to not touch their workstation any further except with guidance from the security team.

A full system scan and quarantine was initiated on all employee’s work devices.

At around 8 pm UTC on February 14, 2024. These scans returned no results, the devices were left in quarantine.
On February 15, 2024: 8:33 PM UTC The fraud prevention team was able to reach the CNI team and confirm details of the incident. At this point the financial institution team was still actively confirming the compromise, and was able to confirm several other access attempts throughout the day.

The final login attempt was around 5 AM UTC. February 16, 2024.

On February 16, 2024 after diligence had been initiated

Once investigation began again on February 16th, CNI security team and employee again spoke with the MFA provider as part of the investigation.

At 4:00 PM UTC February 16, 2024 the CNI security team contacted federal law enforcement, they had reached the end of their paths for investigation and needed to report their findings as well as initiate federal investigation of the alleged crimes.

The employee also spoke with the financial institution again at 5:20 PM UTC February 16th, 2024, who confirmed 2 additional login attempts during that day at 5:11 AM and 8:25 AM UTC respectively.

At 7:20 PM UTC after further investigation in cooperation with law enforcement the fraud prevention team at the bank was able to provide a thorough accounting of all access to the account in question, and were able to match the access to a third party integration, from the financial admin tool, which had been modified back in January.

It was only after careful examination on the financial institutions end, that they established MFA had not been used on this account or the alleged malicious login attempts. It only appeared that way without a fine tooth review of the logins. The financial institution was able to provide an accounting for all activity, and the integration in question was scheduled for removal. CNI security team closed the issue, and law enforcement reports, as a false positive.

## Timeline (all times UTC) 1/29/24, 2/15/24 - 2/16/24 All times approximations

January 29, 2024
CNI finance team established a bank connector with the financial admin tool, which they had been working on for an extended period of time. We have a similar connector with another financial institution. This connector feeds banking transactions into the financial admin tool, which allows us to perform a bank reconciliation within the financial admin tool.  CNI Employee met with our financial admin tool consultant to set-up the connector. This set-up occurred over a Chia Zoom meeting. In order to establish the connection, we logged into financial admin tool and then clicked on the links within financial admin tool to establish a connection. A list of banks popped up and we selected the correct financial institution. From there, CNI Employee entered his login information.
 
**Note that the above information is included for context purposes. Outside of the above login, access to financial institution’s banking portal usually occurs once or twice a month. Additionally, no access to the financial institution has occurred on CNI Employee’s mobile phone and the only work-related apps / websites used on CNI Employee’s mobile phone for Chia is Keybase.
 
February 15, 2024: 7:56am UTC
CNI Employee received an email from financial institution indicating that there was a login from a new device at 2:56am EST.

February 15, 2024: 1:15pm UTC
CNI Employee saw the email from financial institution. He logged into financial institution and banking systems to change passwords. At this time the user's access to CNI systems was disabled.
 
February 15, 2024: 1:23pm UTC
CNI Employee forwarded Justin England the email from financial institution. Justin responded and asked CNI Employee to contact financial institution.
 
February 15, 2024: 3:59pm UTC
CNI Employee contacted financial institution and informed them that he did not access the financial institution banking system at 7:56am UTC. They informed CNI Employee that they would raise the issue to their IT team.
 
February 15, 2024: 5:33pm UTC
CNI Employee received a call from the financial institution bank fraud detection team. They mentioned that someone gained access to the financial institution system using a code, which was sent to a mobile phone with the same last four digits as CNI Employee’s. They were in the system for a very short period of time before financial institution kicked them out of the system. The financial institutions rep confirmed that the IP address used at 7:56am UTC was not the same IP address that was used when CNI Employee logged into financial institution around 1:15pm UTC, rep advised CNI employee to change his password. Rep also said he would forward the IP address used at 7:56am UTC to CNI Employee’s Chia email.
 
February 15, 2024: 6:35pm UTC
CNI Employee called MFA provider and explained the situation. They checked their records for unauthorized access or new logins and detected none. They informed CNI Employee there was nothing else they could do.
 
February 15, 2024: 7:29pm UTC
CNI Employee called mfa provider and explained the situation. They would not provide CNI Employee with additional details because he is not a manager on the account.
 
February 15, 2024: 8:35pm UTC
CNI Employee called mfa provider and was added as a manager on the account.  CNI Employee explained the situation and discussed with the fraud department. They said that it is likely spoofing and there is nothing they can do.
 
February 16, 2024: 3:44pm UTC
Justin England (“Justin”) informed CNI Employee that there was another login attempt to the financial institution banking website the evening of February 15th and asked CNI Employee to call financial institution after the upcoming mfa provider call.
 
February 16, 2024: 4:00pm UTC
CNI Employee called mfa provider with Justin and explained the situation to the support rep. They transferred the call to the technical team. The technical team was unable to pull specific data. The technical team transferred CNI Employee and Justin to the fraud prevention team where they spoke with a rep. The fraud prevention team was also unable to help and suggested CNI Employee and Justin talk with the security legal team.
 
February 16, 2024: 4:33pm UTC
CNI Employee called the mfa provider security legal team and spoke with a rep. She mentioned that there is nothing the legal team can do without a case number through an agency. She suggested Chia file a police report and call back.
 
After the call, federal law enforcement was contacted.
 
February 16, 2024: 5:20pm UTC
CNI Employee called financial institution and spoke with another rep. She mentioned there were two additional login attempts at 5:11am UTC and 8:25am UTC. She contacted the fraud prevention team and mentioned that someone would reach out.

February 16, 2024: 7:20pm UTC
Financial institution fraud prevention contacted CNI employee, and stated after further investigation in cooperation with law enforcement the fraud prevention team at the bank was able to provide a thorough accounting of all access to the account in question, and were able to match the access to a third party integration, from the financial admin tool, which had been modified back in January.

It was only after careful examination on the financial institutions end, that they established MFA had not been used on this account or the alleged malicious login attempts. It only appeared that way without a fine tooth review of the logins. The financial institution was able to provide an accounting for all activity, and the integration in question was scheduled for removal. CNI security team closed the issue, and law enforcement reports, as a false positive.

## Resolution and recovery

The integration in question was scheduled for removal. CNI security team closed the issue, and law enforcement reports, as a false positive.

