# Chia Network Post-Mortem Repository

This repository serves as a comprehensive record of significant incidents, issues, and their resolutions within the Chia Network ecosystem. It provides transparency, facilitates learning from past events, and helps prevent similar issues in the future.

## Purpose

- Document significant incidents and their resolutions
- Provide transparency to the Chia community
- Enable learning from past events
- Guide future incident response
- Track improvements and preventive measures
- Maintain historical record of network evolution

## Repository Structure

```
post-mortem/
├── YYYY-MM/              # Chronological organization by month
│   └── incident-name.md  # Individual post-mortems
├── template.md           # Standard template for new post-mortems
└── assets/              # Supporting materials (graphs, logs, etc.)
```

## How to Use This Repository

### Reading Post-Mortems
- Browse chronologically by date in the YYYY-MM directories
- Use GitHub's search functionality to find specific incidents
- Check the assets directory for supporting materials

### Creating a New Post-Mortem
1. Copy `template.md` to create a new post-mortem
2. Place it in the appropriate YYYY-MM directory
3. Follow the template structure
4. Include relevant assets in the assets directory
5. Submit a pull request

## Post-Mortem Template

Use `template.md` as the base for all new post-mortems. The template includes:
- Issue Summary
- Timeline
- Resolution and Recovery
- Additional Sections (as needed)
- Q&A

## Example Post-Mortem Structure


```markdown
# [Issue Name]

## Issue Summary

*Provide a clear, concise overview of what happened, when it happened, and its impact. Include relevant metrics and scope.*

This is an example issue that occurred between [start date] and [end date]. The incident affected [scope of impact] and resulted in [primary consequences].

## Timeline (all times PST) [Date Range]. All times approximations

*List key events in chronological order. Include specific timestamps when available. Note that times are approximate unless exact.*

- [Time] First indication of the issue
- [Time] Initial investigation begins
- [Time] Root cause identified
- [Time] Mitigation steps initiated
- [Time] Resolution implemented
- [Time] System returns to normal operation

## Root Cause

*Explain the technical or operational cause of the incident. Be specific but avoid unnecessary technical details.*

The issue was caused by [technical description of the root cause]. This led to [specific consequences].

## Resolution and Recovery

*List the steps taken to resolve the issue and restore normal operation. Include both immediate actions and long-term fixes.*

1. Immediate action taken
2. Additional steps implemented
3. System improvements made
4. Monitoring enhancements added

## Impact

*Quantify the impact where possible. Include both technical and business impact.*

- Duration: [time period]
- Scope: [affected systems/users]
- Severity: [impact level]
- Business Impact: [if applicable]

## Preventive Measures

*List specific actions taken to prevent similar incidents in the future.*

1. System improvements implemented
2. Process changes made
3. Monitoring enhancements added
4. Documentation updates completed

## Q&A

*Address common questions about the incident. Focus on community concerns and technical details.*

Q: [Common question about the incident]
A: [Clear, concise answer]

Q: [Another common question]
A: [Clear, concise answer]
```

## Guidelines

- Be thorough but concise
- Include specific timestamps
- Document both technical and user impact
- Provide clear resolution steps
- Include preventive measures
- Add relevant metrics and data
- Cross-reference related incidents

## Contact

For questions about this repository:
- Contact the Chia Network team on the official Discord (Discord:https://discord.gg/chia)
- To report a security issue please use our bug bounty program (https://hackerone.com/chia_network)