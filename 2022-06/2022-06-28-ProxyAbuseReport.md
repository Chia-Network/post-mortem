# Data Layer Testing Proxy Abuse Report

## Issue Summary

On June 28, 2022, Amazon Web Services (AWS) alerted Chia via e-mail that our Data Layer reverse proxy EC2 testing instance had received an abuse complaint.  After investigation, it was determined that the proxy configuration contained an IP address of an EC2 instance that had recently been shut down and the proxy was still forwarding web traffic to this IP.  The IP address had since been reassigned to another customer by AWS and some of the traffic our proxy forwarded was a common vulnerability probe.  The issue was resolved by reconfiguring the proxy with the latest Data Layer testing IP addresses. 

## Involved Parties

Data Layer team: Chia employees and partners working on the Data Layer application.

IPS team: Infrastructure, Platforms, and Security members who responded to the potential incident and collaborate closely with the Data Layer Team.

AWS: Amazon Web Services, the cloud hosting provider. 

## Timeline (all times PST). All times approximations

| Day | Date (2022) | Time (PST) | Event |
|-----|-------------|-----------|-------|
| Mon | Jun 27 | 13:47 | Data Layer team and IPS turn off one Data Layer testing EC2 instance at IP address w.x.y.z (IP address redacted) via the AWS dashboard |
| Mon | Jun 27 | 14:07 | IPS turns back on the Data Layer EC2instance at request of Data Layer team to retrieve data from instance. The instance comes online with a new IP address |
| Tues | Jun 28 | 20:28 | Many requests are made to the Data Layer proxy indicative of a security scan, including requests to `/.env` |
| Tues | Jun 28 | 20:36 | AWS sends an automated email to Chia notifying of an abuse complaint for IP address w.x.y.z who received a request to `/.env` from the IP address of the Data Layer proxy.  This was classified as `exploit:gen/dot_env_leak` by the automated system |
| Tues | Jun 28 | 21:20 | IPS initiates a War-room to coordinate response to the reported incident |
| Tues | Jun 28 | 21:30 | IPS turns off all EC2 instances in the impacted AWS account as a precautionary measure until further investigation can be done |
| Tues | Jun 28 | 21:45 | Outside security experts consulted and on standby to assist |
| Tues | Jun 28 | 21:55 | Reply sent to AWS acknowledging incident
| Tues | Jun 28 | 22:21 | Active incident declared mitigated by the shutdown of the EC2 instances and forensics work to resume in morning |
| Wed | Jun 29 | 7:05 | IPS team begins investigating and finds requests to `.env` files in the proxy logs that match the timestamps of the AWS report |
| Wed | Jun 29 | 10:50 | IPS team confirms IP address that lodged abuse complaint matches IP address of Data Layer testing EC2 instance that had been shut down on Monday Jun 27.  This IP is found in the reverse proxy configuration as it hadn’t been removed when the EC2 instance was shut down and was therefore forwarding traffic to an IP that Chia no longer leased from AWS. |
| Wed | Jun 29 | 11:34 | IPS team emails AWS with explanation of incident |
| Wed | Jun 29 | 11:44 | Data Layer testing nodes are brought back online |
| Wed | Jun 29 | 11:54 | Automation run against Data Layer AWS account to update IP addresses on the proxy and enforce state | 
| Wed | Jun 29 | 12:23 | AWS closes abuse case and marks the issue as solved |
| Wed | Jun 29 | 12:35 | All Data Layer testing nodes confirmed back online with updated routing |

## Root Cause

In the current data layer testing architecture, a single Nginx reverse proxy handles distributing requests to the various testing nodes.  This Nginx server manages SSL certificates and has automated provisioning to automatically update the destination IP addresses of the individual Data Layer EC2 testing instances whenever an instance is created or destroyed.  The automation is only run to create or destroy an instance and if an instance has an IP address change in-between automation runs, the proxy will point to the old address until the automation is run again manually or triggered by a change to the Github repository. 

A number of requests similar to the following are visible in the Nginx logs of the reverse proxy (IP address redacted):

```
x.x.x.x (x.x.x.x) - - [29/Jun/2022:03:28:53 +0000] "GET /.env HTTP/1.1" 200 884 "-" "Mozilla/5.0 (Windows NT 6.3; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/41.0.2226.0 Safari/537.36"
```

The time on these requests matches the time of the AWS abuse report.  The abuse report states the payload is a “exploit:gen/dot_env_leak” which we assume means a request for an unauthorized request for a .env file.  

On Monday, two days prior, we had shut down one of the EC2 testing instances in preparation for eventual decommissioning.  When an EC2 instance is shut down, its IP address is released and reassigned to the AWS pool for reassignment.  The IP address that filed the abuse complaint is the IP address of the instance we shut down Monday, which is the IP address the proxy was still forwarding to for this specific domain name.  

This request for a `.env` file was part of a random security scan originating from a inexpensive cloud server.  These scans are very common on all web-facing IP addresses and do not indicate a compromise or a targeted attack.  Our proxy forwarded these requests to the old IP we no longer owned as the proxy hadn’t been updated when the testing EC2 instance was shut down. 

## Corrective and Preventative Measure

A new shutdown and decommissioning process will be created that removes the IP addresses from the proxy when an EC2 instance is taken offline.  This will likely involve a combination of manual actions (to shut down the instance) and automated actions (to remove the IP from the proxy).  It may be that taking a snapshot and removing the instance entirely from automation (thereby removing it from AWS and the proxy simultaneously) is the best workflow.  

Additionally, these Data Layer test instances are planned for replacement later this year with a new architecture that will not use a proxy and make this no longer relevant. 

