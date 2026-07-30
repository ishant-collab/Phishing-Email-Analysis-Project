# Phishing Email Analysis Project

A hands-on cybersecurity project analyzing real-world phishing email samples using header inspection, MIME/base64 decoding, and threat intelligence tools — built as part of my preparation for SOC Analyst roles.

## About This Project

Phishing remains one of the most common initial attack vectors in real-world security incidents. This project demonstrates a practical, repeatable methodology for triaging and analyzing suspicious emails the way a SOC (Security Operations Center) Analyst would during real incident response.

Each sample is broken down into a structured report covering:
- Email header analysis (sender authenticity, SPF/DKIM/DMARC results)
- Decoded email content and social engineering tactics used
- Extraction and reputation analysis of malicious links
- Indicators of Compromise (IOCs)
- Actionable remediation recommendations

## Methodology

1. **Safe Sample Acquisition** — Real phishing samples sourced from [malware-traffic-analysis.net](https://malware-traffic-analysis.net), a trusted public repository for security research.
2. **Isolated Analysis Environment** — All samples handled inside a Kali Linux virtual machine to prevent any risk to the host system.
3. **Header Analysis** — Inspecting `From`, `Return-Path`, `Received`, and authentication results (SPF/DKIM/DMARC) to identify spoofing.
4. **Content Decoding** — Using Python to parse MIME/base64-encoded email bodies into readable text and HTML.
5. **Link & IOC Extraction** — Identifying embedded malicious URLs without visiting them directly.
6. **Threat Intelligence Verification** — Cross-checking extracted URLs/domains against VirusTotal for reputation data.
7. **Reporting** — Documenting findings in a structured, SOC-style report format.

## Tools Used

- **Kali Linux** (isolated VM) — safe analysis environment
- **Linux CLI** (`unzip`, `cat`, `grep`) — file handling and searching
- **Python 3** (`email` library) — MIME/base64 decoding
- **VirusTotal** — URL/domain reputation scanning

## Reports

| # | Sample | Impersonated Brand | Verdict |
|---|---|---|---|
| 01 | [2025-09-22-Japanese-phishing-email-1800-UTC.eml](reports/phishing-analysis-report-01.md) | ztwenqo.cn | 🔴 Malicious |
| 02 | [2025-09-22-Japanese-phishing-email-1800-UTC.eml](reports/phishing-analysis-report-01.md) | fumious.umfqrit.cn | 🔴 Malicious |

*(More reports will be added as additional samples are analyzed.)*

## Key Findings & Learnings

- A "pass" result on SPF/DMARC does not guarantee an email is legitimate — attackers can configure authentication correctly for domains they themselves control.
- A low detection count on VirusTotal does not mean a URL is safe, especially for newly registered phishing infrastructure that security vendors haven't indexed yet.
- Phishing sites often actively block security scanners (WAF-based evasion) while still serving malicious content to real victims — a strong standalone red flag.

## About Me

Final-year B.Tech CSE student specializing in cybersecurity, seeking SOC Analyst and SDE internship/fresher opportunities.

---
*This project is for educational and portfolio purposes only. All samples were sourced from a public security research repository and analyzed in an isolated environment.*
