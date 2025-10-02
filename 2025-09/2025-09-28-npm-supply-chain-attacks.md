# npm Supply Chain Attacks (September 2025)
## Issue Summary

In September 2025, the npm ecosystem experienced two significant supply-chain compromises affecting widely used packages. On September 8, a maintainer account was phished, and malicious versions of at least 18 high-download packages (e.g., `chalk`, `debug`, `ansi-regex`, `supports-color`, `strip-ansi`) were published, injecting code that attempted to hijack cryptocurrency transactions in browser contexts. Around September 15–16, a second incident (“Shai‑Hulud”) introduced a worm-like postinstall payload that harvested secrets and attempted to republish tainted packages under maintainers’ namespaces, spreading to 100+ additional packages before removal. Community response and registry action limited direct losses, but the events highlighted systemic risks in open-source supply chains.

## Timeline (all times PST) DATE RANGE. All times approximations

- 06:15 AM Sep 08, 2025: First malicious versions of popular npm packages (e.g., `chalk@5.6.1`, `debug@4.4.2`) appear following maintainer account compromise via phishing
- 07:00–08:00 AM Sep 08, 2025: Security researchers flag anomalies; warnings circulate on social media and security vendor feeds
- 10:00 AM Sep 08, 2025: npm begins yanking malicious versions; affected maintainers start revocation and cleanup
- Sep 09–12, 2025: Additional impacted packages identified; advisories and IOCs disseminated
- Sep 15–16, 2025: “Shai‑Hulud” worm campaign publishes malicious postinstall scripts to numerous packages; payload exfiltrates secrets and attempts self‑propagation
- Sep 16–18, 2025: Registry takedowns; organizations rotate tokens and audit pipelines; expanded impacted-package lists published

## Resolution and recovery

- Tainted package versions were removed from npm; owners reclaimed accounts and published clean releases
- Maintainers and downstreams rotated npm/GitHub/cloud credentials; CI agents and caches were purged and rebuilt
- Organizations audited dependency locks, rolled back to known‑good versions, and implemented temporary blocks on install scripts
- Security advisories, IOCs, and impacted package/version lists were shared across the community to accelerate triage

### Root cause
- Human‑targeted phishing defeated a maintainer’s MFA via an adversary‑in‑the‑middle flow, yielding a valid session to publish malicious updates
- For the worm campaign, compromised tokens and weak credential hygiene enabled propagation: postinstall payloads harvested secrets and used them to push trojanized patch releases under maintainers’ namespaces

### Impact
- While Chia network was not impacted, Chia network did have a near miss where we would have installed one of the common vulnerable dependencies had we not been alerted and checked pending package updates for all our repos. We did not install a vulnerable depdency but the near miss factor alerted us to some possible flaws in our process and we have taken steps to correct them. 

### Detection
- Community monitoring, vendor intel feeds, and social alerts (including maintainers and security researchers) identified anomalous diffs and behaviors quickly


### Preventative and corrective actions
- Enforce phishing‑resistant MFA (FIDO2/WebAuthn) for npm/GitHub maintainers and high‑impact publishers
- Pin dependencies with lockfiles; prefer `npm ci` in CI; avoid floating ranges in production builds
- Block install‑time scripts by default in CI (`npm config set ignore-scripts true`) and enable selectively per allowlist
- Use SCA and malware scanning pre‑install (e.g., quarantine obfuscated diffs, network hooks, or wallet/API interception code in utility packages)
- Segregate and minimize tokens; rotate credentials after incidents; scope tokens to least privilege
- Periodically purge private registries’ caches to prevent serving yanked upstream artifacts
- Generate and retain SBOMs to rapidly locate affected builds for rollback
- We have implemented a more robust tool chain for repository based languages and protocols that include pinned versions of dependencies and enhanced monitoring and upgrade practices. 

## Q&A

Q: Were we affected?
A: Neither Chia network nor the blockchain were impacted. We recognized lessons learned from this massive NPM community issue and have improved our process and practices to ensure no future vulnerabilities exist.

Q: What about end‑users?
A: No Chia Blockchain or other end users were impacted 

Q: What’s the likelihood of recurrence?
A: High, given attacker ROI. Expect continued phishing of maintainers and token theft targeting postinstall vectors and CI.

Q: What long‑term measures matter most?
A: Phishing‑resistant MFA for maintainers, strict dependency hygiene, script blocking in CI, token hardening, and continuous supply‑chain monitoring. We have taken steps to improve our process around dependencies and feel we have create a more secure pattern around installation and life cycle management that manages all new information from this attack and its analysis. 

---

Sources (evidence of compromises and analysis):
- Aikido: “S1ngularity/nx attackers strike again” (Sept 16–19, 2025) – details worm‑like propagation, secret exfiltration, impacted packages: https://www.aikido.dev/blog/s1ngularity-nx-attackers-strike-again
- Security Boulevard (Legit Security syndicated): “Shai‑Hulud npm Attack: What You Need to Know” (Sept 18, 2025) – attack mechanics, impacted packages list: https://securityboulevard.com/2025/09/shai-hulud-npm-attack-what-you-need-to-know/
- DEV posts and community write‑ups documenting Sept 8 compromise of `chalk`, `debug`, and related packages, crypto‑drainer payload behavior, and download scale:
  - https://dev.to/figsify/anatomy-of-a-supply-chain-heist-the-day-chalk-and-debug-became-crypto-thieves-4fb
  - https://dev.to/aerabi/the-largest-npm-supply-chain-attack-ever-and-how-to-defend-against-it-9a6
- FirstPoint: “Securing Repos After the September 2025 NPM Supply Chain Attack” – summary and malicious version matrix: https://blog.firstpoint.com.tr/securing-repos-after-the-september-2025-npm-supply-chain-attack/
- Additional ecosystem advisories and summaries corroborating Sept 8 and Sept 15–16 activity windows:
  - Posit Support advisory: NPM Supply Chain Compromise – Sept 2025
  - Guardrail.ai, Blockaid, and other vendor analyses of crypto‑drainer payloads and response guidance