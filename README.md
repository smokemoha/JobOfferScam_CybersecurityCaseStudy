# Case Study: Verifying a Suspicious Remote Job Offer (Airspan “Data Entry Clerk”)

/JobOfferScam_CybersecurityCaseStudy/
├── README.md 
├── artifacts/
│   ├── WARNING.md 
│   ├── original_offer_letter.pdf 
│   └── virustotal_screenshots/
│       ├── Screenshot_541.png
│       ├── Screenshot_542.png
│       ├── Screenshot_543.png
│       └── Screenshot_544.png
└── documentation/
    ├── comprehensive_report.md
    ├── malware_analysis.md
    └── research_findings.md

Author: Sadisu Mohammed
Date: 08 19, 2025
Role: SOC Analyst (aspiring Red Teamer)

## Executive Summary
I investigated a remote “Data Entry Clerk” job offer allegedly from Airspan Networks. While the PDF offer letter returned 0/64 detections on VirusTotal, deeper verification and behavioral analysis revealed multiple red flags consistent with a job-scam/social-engineering campaign. Conclusion: the posting and offer were fraudulent.

## Objective
Demonstrate a repeatable process a cybersecurity engineer would use to validate a job posting, assess attached artifacts, and document conclusions for stakeholders.

## Timeline
- T0: Unsolicited email with interview link and PDF offer letter
- T1: Chat interview over Microsoft Teams; immediate offer issued
- T2: Static scan (VirusTotal) → 0/64 detections
- T3: Deeper verification (company careers site, sender identity, behavior view) → Multiple red flags

## Evidence Collected
- Original email (headers captured)
- Offer letter PDF (hash: b89bb5ce4dde4fed2f2ba8c01dc5d9699d6363343e7865f275fe65ea9f7f9fc3)
- VirusTotal screenshots (Detection, Details, Behavior)
- Screenshot of third‑party “urgent hiring” ad using non-corporate address
Note: SHA-256 hash is provided for educational purposes only; the file analyzed is a known scam artifact and contains no sensitive personal data.

## Verification Steps & Findings
### 1) Sender and Channel Hygiene
- From address: infoairspan@gmail.com → non-corporate domain
- Legitimate enterprises use @airspan.com for recruiting. Non-matching domains are a high-confidence indicator of impersonation

### 2) Role Cross‑Check
- No “Data Entry Clerk” role listed on Airspan’s official careers page during the investigation window
- Message asked to contact a “hiring manager” whose identity and address did not map to Airspan HR

### 3) Compensation & Process Sanity
- $25/hour for entry-level remote data entry is materially above typical ranges; immediate offer post chat and pressure (“limited interview slots”) → classic scam pattern

### 4) Artifact Analysis (PDF Offer Letter)
- VirusTotal Detection: 0/64 → not proof of safety; signatures miss novel or non-binary threats
- VirusTotal Behavior (sandbox):
  - File system actions touching Adobe Reader data paths; benign PDFs should not write arbitrary files
  - Process invocations with unusual parameters (AcroCEF flags) consistent with staged execution or environment probing
  - Highlighted API: GetTickCount64 → commonly used for anti-analysis/anti-VM timing checks

Interpretation: While the PDF may not carry a detectable payload, the operational flow relies on social engineering to progress to data harvesting or follow-on actions (e.g., payroll setup, equipment purchase scams, credential capture).

## Conclusion
The offer is fraudulent. Indicators include non-corporate sender domain, absence of the role on the official careers site, rushed informal process, inflated pay, and suspicious behavioral traces observed in sandboxing.

## What I Did Well (Skills Demonstrated)
- OSINT and source validation (careers site, company identity)
- Email/headers hygiene review and judgment
- Sandbox behavior triage (beyond signatures) and API significance (GetTickCount64)
- Clear documentation and risk communication

## What I Would Improve Next Time
- Detonate in an isolated VM to capture endpoint telemetry (Sysmon/EDR) and registry/file artifacts
- Perform controlled network capture (tcpdump/Wireshark) during open → look for callbacks
- Automate VT lookups and artifact hashing in a repeatable playbook

## Actionable Checklist (Repeatable Playbook)
- Verify sender domain against official site and MX/SPF/DMARC posture
- Cross-check the role on the official careers portal
- Validate recruiter identity on LinkedIn/Company HR pages
- Hash all attachments; scan with VT; review Behavior tab, not just Detection
- If needed, detonate safely in a VM with no credentials or mapped drives
- Document red flags, artifacts, and hashes; recommend user actions (cease contact, AV scan, password hygiene, reporting)

## Recommended User Actions (for any victim)
- Stop contact, report impersonation to the real company and local cybercrime unit
- Full AV/EDR scan; change reused passwords; enable MFA
- Monitor accounts for unusual activity

## Appendix A: Sanitized Email Header Highlights
- From: infoairspan@gmail.com [REDACTED]
- DKIM/SPF/DMARC: pass for gmail.com (proves it’s from Gmail, not that it’s from Airspan)

## Appendix B: Hashes
- PDF SHA‑256: b89bb5ce4dde4fed2f2ba8c01dc5d9699d6363343e7865f275fe65ea9f7f9fc3
- Disclaimer: Hash is provided for educational and threat intelligence purposes only; no personal data is associated with the shared artifact.

## Appendix C: Learning Notes (My Personal Voice)

As an aspiring red teamer with a SOC analyst background, this incident was an eye-opening experience. Initially, seeing 0/64 detections on VirusTotal was almost reassuring, highlighting my early reliance on automated tools. However, diving into the behavioral analysis section was a game-changer. It truly drove home the critical lesson that \'undetected\' does not mean \'safe,\' and that understanding a file\'s actions is paramount. This investigation solidified my appreciation for the defender\'s mindset in dissecting threats and reinforced my motivation to understand adversarial tactics for red teaming. Moving forward, I\'ll prioritize behavioral analysis in my threat assessments and look for opportunities to automate initial evidence collection and sandbox analysis for quicker insights.

---

