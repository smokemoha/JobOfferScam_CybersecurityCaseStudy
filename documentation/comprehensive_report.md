# Comprehensive Investigation Report: Analysis of a Suspicious Job Offer

## Executive Summary

This report details the findings of a cybersecurity investigation into a suspicious job offer received by an aspiring red team professional. The offer, which the user applied for after seeing an online posting, purportedly from "Airspan Networks" for a "Data Entry Clerk" position, exhibited numerous red flags indicative of a scam, including a switch in email domains from the initial application, an unusually high salary for the role, and an informal hiring process. While a VirusTotal scan of the attached employment offer letter (PDF) showed no direct malware detection, behavioral analysis revealed suspicious file system and process activities. This incident serves as a critical case study in multi-stage social engineering and highlights the importance of vigilance, thorough verification, and advanced behavioral analysis in identifying and mitigating potential cyber threats.

### 1.1 Job Offer Email Analysis

The user initially applied for a "Data Entry Specialist" position after seeing an online posting, which directed applications to `mohamed.mostafa@aljouf.ly`. Subsequently, the user received an email (sadisumohammed27@gmail.com) from `infoairspan@gmail.com`, congratulating them on being shortlisted. This switch in email domains from the initial application point to a generic Gmail address immediately raised a significant red flag, as legitimate corporations typically utilize their official domain for professional communications, especially for sensitive matters such as job offers, rather than a generic email service like Gmail. The content of the email was broadly generic, directing them to contact a Hiring Manager via Microsoft Teams using a specific verification code (ASP-005). This lack of formal communication channels and the domain switch are common characteristics of sophisticated phishing and scam attempts.

### 1.2 Employment Offer Letter (PDF) Analysis

The PDF document, titled "Sadisu Mohammed EMPLOYMENT OFFER LETTER FULL TIME-AIRSPAN.pdf," presented itself as a formal offer letter for a Data Entry (Full Time) position at AIRSPAN. Key details extracted from the document included:

- **Company Address:** 5201 Congress Ave, Suite 130, Boca Raton, FL 33487, USA.
- **Recipient Address:** 42 Morningside Avenue, Johannesburg Gauteng, 2196 (corresponding to the user's location).
- **Proposed Start Date:** August 25, 2025.
- **Work Arrangement:** Initially remote, with a future option for office work once suitable premises are established.
- **Working Hours:** A minimum of 35 hours per week, with flexible shift options (8:30 AM to 4:30 PM or 4:30 PM to 11:00 PM).
- **Compensation:** A rate of $25 per hour post-training, and $15 per hour during a two-week training period.
- **Payment Frequency:** Weekly payments on Fridays.
- **Probationary Period:** A one-month probationary period.
- **Benefits:** Inclusion of Superannuation Guarantee payments, 10 paid personal/career's leave days, 2 unpaid career's leave days, and 2 paid compassionate leave days.
- **Confidentiality Clause:** Standard confidentiality provisions concerning company information.
- **Governing Law:** Stated as the State of Florida, USA.

Despite the seemingly professional formatting of the letter, the initial contact via a Gmail address significantly undermined its credibility. The offer of a high hourly wage for a data entry position, coupled with the promise of flexible hours and remote work, is a frequently observed tactic in employment scams designed to attract and deceive unsuspecting individuals.

### 1.3 VirusTotal Analysis of the PDF

The VirusTotal analysis, as depicted in the provided screenshots, indicated that the PDF file itself was not flagged as malicious by any of the 64 security vendors, resulting in a 0/64 detection score. This suggests that the document is not a direct carrier of known malware signatures. However, a deeper examination of the behavioral analysis provided by VirusTotal revealed several highly suspicious activities, underscoring the principle that the absence of signature-based detection does not equate to the absence of a threat. Modern cyber threats often leverage social engineering, fileless techniques, or exploits that bypass conventional antivirus solutions.

**File System Actions:** The behavioral analysis showed the PDF initiating interactions with the file system that are atypical for a benign document. Specifically, it attempted to access and potentially write data to directories associated with Adobe Acrobat Reader, such as `SOPHIAReader/TESTING` and `SOPHIAReader/SOPHIA.json`. The observation of "files being dropped" in these locations is particularly alarming. In a legitimate operational context, a standard PDF document should not be writing executable files or configuration data to arbitrary system locations. This behavior could be indicative of:

- **Dropper Functionality:** The PDF might be designed to deploy and execute a secondary malicious payload onto the system. Even if the PDF itself is not inherently malicious, it could serve as a conduit for delivering other harmful components.
- **Persistence Mechanism:** The dropped files could be part of a mechanism to establish persistence, allowing an attacker to maintain unauthorized access to the compromised system across reboots.
- **Information Gathering:** The `SOPHIA.json` file, if created, could be used to collect sensitive system information or user data for subsequent exfiltration.

**Process and Service Actions:** While the execution of `Acrobat.exe` is expected when opening a PDF, the associated shell command `C:\Program Files\Adobe\Acrobat DC\Acrobat\AcroCEF\AcroCEF.exe" --background-color=16514043` is unusual. Although `--background-color` is a valid command-line argument for some applications, its specific presence in this context, particularly with a distinct hexadecimal color code, could represent an attempt to:

- **Obfuscate Malicious Activity:** By manipulating the background color or other visual elements, an attacker might aim to divert the user's attention or conceal pop-up windows related to malicious operations.
- **Exploit Vulnerabilities:** In certain scenarios, unconventional command-line arguments can be leveraged to trigger vulnerabilities within the application, potentially leading to arbitrary code execution.

**Runtime Modules Loaded:** The loading of numerous Dynamic Link Libraries (DLLs) is a normal occurrence for a complex application like Adobe Acrobat. However, a thorough analysis by a red team professional would involve scrutinizing this list for any anomalous or unexpected DLLs that might suggest code injection or the loading of malicious libraries. Without a verified baseline of normal behavior for this specific version of Adobe Acrobat, it is challenging to definitively identify anomalies solely from this list. Further dynamic analysis within a controlled environment would be essential to pinpoint any suspicious DLLs.

**Highlighted Actions: `GetTickCount64`:** The `GetTickCount64` API call, while innocuous in isolation, is frequently employed by malware for various purposes:

- **Anti-Analysis/Anti-VM:** Malware often utilizes timing functions such as `GetTickCount64` to detect if it is operating within a virtual machine or a sandboxed environment. If the elapsed time is unusually short or excessively consistent, the malware might infer that it is under analysis and consequently alter its behavior or terminate to evade detection.
- **Delay Execution:** This function can be used by malware to introduce delays before executing its malicious payload, a common technique to bypass time-limited sandbox analyses.
- **Randomization:** It can also serve as a seed for random number generators, which are used in various malicious activities.

**Summary of Initial Findings:** While the PDF itself was not directly detected as malware by antivirus engines, the behavioral analysis strongly indicates that opening the document triggers a series of suspicious actions, including attempts to write files and execute a shell command with unusual parameters. This behavior, combined with the highly suspicious origin of the email, necessitates further investigation. The fact that the user was instructed to download and communicate via Microsoft Teams, rather than through a formal company communication channel, constitutes another significant red flag.

## 2. Research Findings: Airspan Company Legitimacy and Job Offer Verification

To ascertain the legitimacy of the job offer, comprehensive research was conducted on Airspan Networks and common job scam tactics. The findings reveal significant discrepancies that strongly suggest the offer is fraudulent.

### 2.1 Airspan Company Verification

- **Official Website:** The official website for Airspan Networks is confirmed to be `airspan.com`. The website presents a professional image, providing extensive information about their products, services, and corporate profile. Crucially, the contact information listed on the official website, including the headquarters address (5201 Congress Ave, Suite 130, Boca Raton, FL 33487, USA), precisely matches the address provided in the suspicious offer letter. This similarity is a common tactic used by scammers to lend an air of authenticity to their fraudulent schemes.
- **Job Postings:** A thorough review of the "Careers" section on the official Airspan website was conducted. While numerous legitimate job openings were listed, primarily for engineering and technical roles across various global locations (e.g., India, USA), there was a critical absence of any listing for a "Data Entry Clerk" position. This discrepancy is a major red flag, as legitimate companies consistently advertise their open positions on their official career portals.

### 2.2 Common Job Scam Tactics

Research into prevalent job scam tactics, particularly those targeting remote data entry positions, identified several patterns that align directly with the user's experience:

- **Unrealistically High Pay:** The offer of $25/hour for a data entry position is considerably higher than the typical market rate for such roles, especially for remote work. This inflated compensation is a classic bait used by scammers to attract victims and bypass their critical judgment.
- **Generic Job Descriptions:** The job description provided during the interview chat was notably vague, lacking specific details regarding the software, tools, or precise responsibilities involved. Legitimate job descriptions are usually detailed and specific.
- **Unprofessional Communication:** The use of a generic Gmail address (`infoairspan@gmail.com`) for official communication is a glaring red flag. Reputable companies invariably use their own corporate email domains for all official correspondence, particularly for recruitment and job offers.
- **Immediate Job Offer:** The user received a job offer remarkably quickly after a brief online chat interview. Legitimate hiring processes are typically more rigorous, involving multiple interview stages, background checks, and a more extended evaluation period.
- **Requests for Personal Information (Implied):** While not explicitly detailed in the provided information, these types of scams frequently progress to requests for sensitive personal and financial information (e.g., bank account details for payroll, social security numbers) under the guise of onboarding or equipment procurement.

### 2.3 Email Sender Investigation

An investigation into the email address `infoairspan@gmail.com` did not yield any official links or associations with Airspan Networks. Search results predominantly pointed to general information regarding Gmail security and common scam methodologies. This further solidifies the suspicion that the email did not originate from a legitimate Airspan representative.

### 2.4 Conclusion of Research

Based on the comprehensive research, it is overwhelmingly evident that the job offer is a scam. The confluence of an unadvertised job position on the official company website, the use of a non-corporate Gmail address for official communication, an excessively high salary for the role, and an expedited, informal hiring process are all definitive indicators of a fraudulent scheme. This incident is a textbook example of an employment scam designed to exploit individuals seeking remote work opportunities.
