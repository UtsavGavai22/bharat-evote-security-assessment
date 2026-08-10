# Bharat E-Vote — Application Security & Penetration Testing

## Overview

This repository documents an authorized web application security assessment
of **Bharat E-Vote** (`https://bharat-evote.me`), conducted with the
knowledge and consent of the site owner. The goal of the engagement was to
identify vulnerabilities in the application's authentication, session
management, file handling, and web server configuration, and to provide
actionable remediation guidance.

> ⚠️ **Status:** This assessment targets a live, production application.
> Findings in this repository are disclosed responsibly and in coordination
> with the site owner. If you are the owner of bharat-evote.me and have
> concerns about any content here, please reach out via the contact info in
> the engagement records.

## Engagement Summary

| | |
|---|---|
| **Target** | `https://bharat-evote.me` |
| **Type** | Authorized black-box / grey-box web application penetration test |
| **Tester** | [Utsav Gavai](https://github.com/UtsavGavai22) |
| **Authorization** | Confirmed with site owner prior to testing |
| **Status** | Findings reported to owner; remediation tracked in `Remediation.md` |

## Repository Structure

```
.
├── README.md          - this file: project overview and scope
├── Methodology.md      - testing methodology and approach
├── Findings.md         - vulnerabilities identified (disclosed responsibly)
├── Remediation.md      - recommended fixes for each finding
├── tools.md             - tools used during the assessment
├── scope.md             - what was tested and what was explicitly excluded
└── screenshots/          - non-sensitive supporting evidence
```

## Disclosure Policy

This assessment follows a **coordinated / responsible disclosure** approach:

1. All findings were reported privately to the site owner first.
2. Proof-of-concept detail sufficient to *reproduce and exploit* each issue
   is withheld from this public repository until the owner confirms a fix is
   deployed (or grants explicit permission to publish in full).
3. Findings below describe the vulnerability class, impact, and evidence at
   a level useful for other engineers/learners, without functioning as a
   ready-to-use attack guide against the live site.
4. No real user credentials, session tokens, personal voter data, or other
   sensitive production data are included anywhere in this repository.

## Disclaimer

This work was performed for educational and authorized security-assessment
purposes only. Do not use any information in this repository to test or
attack `bharat-evote.me` or any other system without explicit written
authorization from the system owner. Unauthorized access to computer
systems is illegal in most jurisdictions.
