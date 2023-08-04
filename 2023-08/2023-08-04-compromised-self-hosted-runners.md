# Compromised self-hosted runners.

## Issue Summary

Chia uses Github Actions for its continuous integration (CI) and continuous delivery (CD).
Host machines that execute jobs of Github Actions Workflows are called runners, and these can either be hosted by Github or self-hosted; i.e. Chia maintains a machine in a datacenter that executes the workflows and uses self-hosted runners that are non-ephemeral, meaning after they complete a task, they move onto the next.

Workflow jobs have many functions: running tests, benchmarks, test coverage, signing and deploying packages, and more. This means that they need to have access to github secrets, sensitive information such as access tokens, and also signing certificates that are used to sign the Chia software to prove these are Chia Network Inc. approved artifacts. This information is siloed based on github permissioning.

When a Chia github repository is forked, workflows can be modified and executed on Chia’s self-hosted runners if criteria are met that are set in github organization/repository settings.
The setting that was enabled at the time of the compromise allowed a contributor to Chia-Network/chia-blockchain (meaning they have previously submitted a PR that was approved and merged) to modify a workflow in their fork and configure it to run on the self-hosted runner attached to Chia-Network/chia-blockchain when they open a pull request to Chia-Network/chia-blockchain.

These workflows can be used to obtain persistence on the self-hosted runner and steal secrets from or tamper with subsequent builds.

Chia strongly believes the community owns the blockchain codebase and tries to balance the ease of community contributions to the codebase while keeping it secure. In this case, the github configuration setting inadvertently led to the compromised self-hosted runners.


## Timeline (all times PST) 07/27/2023 - 07/31/2023. All times approximations

07-27-2023 2:14 pm - Attacker’s PR was approved, making him a contributor via
https://github.com/Chia-Network/chia-blockchain/pull/15877

07-29-2023 11:51 am - Unauthorized access to the runners was gained via https://github.com/Chia-Network/chia-blockchain/pull/15896

07-30-2023 2:42 am - Chia changed the github setting from [Require approval for first-time contributors] to [Require approval for all outside contributors] for Chia-Network/chia-blockchain.

07-30-2023 2:55 am - Chia changed the github setting from [Require approval for first-time contributors] to [Require approval for all outside contributors] for Chia-Network organization.

07-30-2023 3:22 am - Chia reported the github user to github support for malicious probing behavior.

07-31-2023 9:13 am - a bugcrowd report detailing the self-hosted runner compromise was submitted.

07-31-2023 11:00 am - Chia confirmed the bugcrowd issue and engaged with the attacker to evaluate its impact and start remediation. 

07-31-2023 11:20 am - all self-hosted runners were stopped, removing the gained unauthorized access.



## Resolution and recovery

Chia has been in contact with Adnan Khan, the researcher that submitted the bugcrowd report and gained access to the self-hosted runners. In addition to the detailed bugcrowd report, he has also provided a post-action summary.

Chia plans to have a 3rd party security company perform a “red team” (researchers trying to overcome cyber security controls) assessment, based on the generous donation of time Adnan Khan put into providing Chia information on the attack. 

Chia Network is actively working on getting the compromised hosts rebuilt and back into working order.

All certificates and secrets are being rotated. While professional researchers typically do not access the secrets on bug bounty systems and we only have evidence confirming a small portion of secrets being at risk, we are proceeding as if all secrets were exfiltrated. This means new builds from Chia’s continuous integration (CI) should be viewed with a skeptical eye from a signed installer perspective until new signature certificates have been rotated in. 

The procedure for rotating certificates is different per Operating System; Apple certificates are being updated at time of writing. The Linux ecosystem differs from Apple and Windows and is already re-secured. Windows certificates are pending at this time, because of an industry wide technology change in certificate management that required a rebuilding of some internal build time patterns.  

All other Chia Network Inc secrets are also being rotated and replaced with new as well. 

In addition to secret rotation work there are also changes coming to how our runners are managed to get away from static (or non ephemeral) runners wherever possible. Further segmentation of internal and public facing hosts is also under way. Our goal is to remove as much attack surface as possible with these upgrades.



