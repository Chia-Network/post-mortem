# hsmwizard utility unsanitized user input

## Issue Summary

The hsms.cmds.hsmwizard.py script within the Chia-blockchain application is vulnerable to an OS Command Injection.
This allows an attacker to execute arbitrary shell commands on the system running the hsmwizard utility due to unsanitized user input.

An attacker will need to have access to the host where hsms.cmds.hsmwizard.py resides and sufficient privileges to execute the commands, making Social Engineering attacks the most likely path to exploit this issue.

The utility hsmwizard resides on a HSM (a physical device that safeguards and manages cryptographic keys) used by Chia, that is air gapped at secured locations and are only interacted with by approved senior people at Chia.

Chia has not encountered attempts to leverage the hsmwizard issue in an attack itself.
This post-mortem serves as a disclosure and means to give credit to the researcher for reporting the issue so that others that would use the hsmwizard tool
are aware of the issue.


## Timeline (all times PST) 06/10/2025. All times approximations

06-10-2025 - The issue was reported by Goro Matsumoto via a HackerOne (https://hackerone.com/chia_network/) submission.


## Resolution and recovery

This post-mortem serves as a disclosure for others than CNI that may use the hsms.cmds.hsmwizard.py script.
