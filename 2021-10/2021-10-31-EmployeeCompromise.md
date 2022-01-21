# Suspected Chia Employee Compromise

## Issue Summary

__Type of Incident Detected__: Unexplained spend of one mojo and unexplained commands found in history.

__Initial Concerns__: On Sunday, October 31 at 21:33 PT, a user discovered he had one mojo deducted from his wallet that he does not remember spending. Due to the ongoing Dust Storm attack, this caused concern. Upon more investigation, we also discovered unexplained commands in the history that the user did not initially recall using.

## Timeline (all times PT )

Date/Time Stamps (Captured from keybase messages, calls, and face-to-face communication)

| Day | Date (2021) | Time (PT) | Event |
|-----|-------------|-----------|-------|
| Sun | Oct 31 | 21:33 | Affected user posts in Keybase, “interesting. My machine sent out 1 mojo to somewhere which I know I didn’t do.”
| Sun | Oct 31 | 21:34 | Chia employee starts troubleshooting with a via Keybase direct messages
| Sun | Oct 31 | 21:35 | User claims he hasn’t created any Plot NFT: “It’s my old Machine I recently turned it back on. Trying to figure out how this happened since this is using og plots and never joined any pools”
| Sun | Oct 31 | 21:43 | Verified 1 mojo did leave via xchscan.com
| Sun | Oct 31 | 21:46 | Verified running latest Chia software 1.2.10
| Sun | Oct 31 | 21:53 | Verified git commit hash for is correct
| Sun | Oct 31 | 21:54 | Employee asks the user to shut down his machine.
| Sun | Oct 31 | 21:54 | Employee brings in Sr. Director of Devops to the conversation.
| Sun | Oct 31 | 22:17 | It was verified that no nefarious community PR was checked into Chia software 1.2.10
| Sun | Oct 31 | 22:32 | User recalls the computer was also used for a different blockchain game.
| Sun | Oct 31 | 22:43 | User is asked to to run “history” of past shell commands
| Sun | Oct 31 | 22:52 | User posts 4 screen shots of his “history”
| Sun | Oct 31 | 22:59 | The employee notices there were commands listed after the machine was shut down
| Sun | Oct 31 | 23:05 | Agreement is made that there is enough suspicion on the user’s machine to require digital forensics
| Sun | Oct 31 | 23:16 | It is identified that the user uses shared internet from the apartment complex
| Sun | Oct 31 | 23:17 | A recommendation is provided for the user to have his significant other report the security incident to her employer
| Sun | Oct 31 | 23:25 | The Chia employee is approved to fly to the user’s location
| Mon | Nov 1 | 17:00 | The employee starts setting up brand-new hardware for the user
| Mon | Nov 1 | 17:32 | The employee asks the user to document all actions performed leading up to the event and recommends watching videos on creating a Plot NFT
| Mon | Nov 1 | 17:32 | Confirmation is received from the user’s significant other’s company that there was no compromise of her work equipment
| Mon | Nov 1 | 19:10 | In Keybase, the user writes: “The command you told me to run, I ran it in a separate command line window, and then I closed out my client. This may be the reason what happened to the command line order. I took a look at the command line history, those are the exact commands I ran when I turned the machine on last friday.”
| Mon | Nov 1 | 20:56 | The Sr. Director of DevOps concurs that the employee can do some initial digital forensics on the suspected system
| Mon | Nov 1 | 21:54 | The employee verified the security incident was a False Positive and has reproduced the problem twice. It was also determined that the user did create a Plot NFT



## Root Cause

It was discovered that a Plot NFT was created, explaining how the one mojo was spent. Additionally, the user had two terminal windows open, one running the Chia GUI from CLI and another one typing the unexplained commands. When you power down the machine without closing the terminal windows or Chia GUI, Ubuntu arranges the history out of order.



## Resolution and recovery

The workforce members responded quickly, while also responding to another workplace incident. The time between the end-user reporting the incident until verifying it was a false positive took approximately twenty-four hours. This included rapid replacement of new equipment and having remote hands onsite, completing initial digital forensics.

## Corrective and Preventative Measure

Develop better techniques in questioning the end-user, allowing for prompt detection of the security incident as a false positive.
Hire a professional SecOps person and create formal processes/guidance for security incidents.
Add a feature to the Chia Graphical User Interface (GUI) to proactively inform the user that one mojo will be spent to create a Plot NFT.
