# Phishing Email Analysis Report #01

**Analyst:** Ishant
**Date of Analysis:** [31-July_2026]
**Sample Source:** malware-traffic-analysis.net (2025-10-06-Japanese-phishing-emails-208-examples)
**Sample File:** 2025-09-22-Japanese-phishing-email-1800-UTC.eml

---

## 1. Executive Summary

This email impersonates **Yodobashi.com**, a well-known Japanese electronics retailer, in order to trick recipients into clicking a fraudulent "verify your account" link. The email uses urgency and fear tactics (claiming suspicious account activity, threatening a 72-hour account suspension) to pressure the victim into acting without thinking. Based on header analysis, domain mismatch, and sandbox testing, this email is confirmed as **malicious phishing**.

**Verdict: 🔴 MALICIOUS**

---

## 2. Email Header Analysis

| Field | Value | Observation |
|---|---|---|
| From | `info.ihvosjjm@ztwenqo.cn` (displayed as "ヨドバシ・ドット・コム") | Sender domain has no relation to the real Yodobashi domain (yodobashi.com) |
| Return-Path | `info.ihvosjjm@ztwenqo.cn` | Matches From, but domain itself is attacker-controlled |
| Received (sending server) | `hfuzuhio.cn` via `googleusercontent.com [34.84.5.251]` | Sent from a rented Google Cloud server, not a legitimate corporate mail server |
| DKIM | `none` | No email signature present — cannot verify authenticity |
| SPF | `pass` | SPF passed only because the attacker configured it for their own fake domain (`ztwenqo.cn`) — this does NOT mean the email is legitimate |
| DMARC | `pass (policy=none)` | Policy is set to "none," meaning no enforcement action is taken even on failure |
| Server-side flag | `X-Recommended-Action: reject` | The receiving mail server itself flagged this email for rejection |

**Key takeaway:** SPF/DMARC "pass" results can be misleading — they only confirm the email came from a server authorized for *that specific sending domain*, not that the domain itself is trustworthy. Here, the attacker registered their own domain and configured SPF/DMARC for it, which still allows a "pass" while impersonating a real company in the display name and body content.

---

## 3. Email Content Analysis

**Subject (translated):** "Yodobashi.com: Notice regarding your customer information change request"

**Body summary (translated):**
> "Thank you for using Yodobashi.com. As of 2025/9/23, suspicious access was detected on your account. For security purposes, please log in via the link below to verify your information. If you do not respond within 72 hours, your account may be temporarily suspended."

### Social Engineering Tactics Identified

| Tactic | How It Was Used |
|---|---|
| Brand impersonation | Uses the name and branding style of a trusted, well-known retailer |
| Urgency/fear | "72-hour" deadline with threat of account suspension |
| Unverified claim | Vague reference to "suspicious access" with no real evidence |
| Fake legitimacy | Includes a fabricated copyright line and customer support email to appear authentic |
| Call-to-action button | A prominent "Confirm Account" button designed to be clicked without scrutiny |

---

## 4. Malicious Link Analysis

**Extracted link (from HTML body):**
```
https://fumious.umfqrit.cn/login_index/
```

### Indicators of a Fraudulent Link

| Indicator | Detail |
|---|---|
| Domain mismatch | Real Yodobashi domain is `yodobashi.com`; this link uses an unrelated domain (`umfqrit.cn`) |
| Random subdomain | `fumious.` has no relation to the brand — consistent with auto-generated phishing infrastructure |
| Wrong country TLD | `.cn` (China) domain used to impersonate a Japanese company |
| Generic path | `/login_index/` is a common template path used across phishing kits |

### Additional Finding: Tracking Pixel
The email also contains a 1x1 invisible tracking image (`width=1 height=1`), commonly used by attackers to confirm when a victim opens the email — this is a technique also used in legitimate email marketing, but combined with the other indicators here, it supports tracking of victim engagement for the phishing campaign.

---

## 5. Sandbox / Reputation Check (VirusTotal)

| Field | Result |
|---|---|
| URL Scanned | `https://fumious.umfqrit.cn/login_index/` |
| Detection Score | 1 / 98 security engines flagged as malicious |
| HTTP Status | 403 |
| Tag | `blocked-waf` |

**Analysis:** Although only 1 of 98 engines flagged this URL, this is explained by two factors:
1. The domain is newly registered/used specifically for this campaign (as of the email's send date), so most reputation databases had not yet indexed it.
2. The site's Web Application Firewall (WAF) actively blocked VirusTotal's scanner (`403 blocked-waf`), preventing automated tools from fully inspecting the page. This is a known evasion technique used by phishing infrastructure to avoid detection by security crawlers while still serving the malicious page to real victims.

**A low detection count should never be interpreted as "safe" in isolation** — it must be considered alongside all other indicators (domain mismatch, header anomalies, urgency tactics, WAF blocking behavior).

---

## 6. Indicators of Compromise (IOCs)

| Type | Value |
|---|---|
| Sender email | info.ihvosjjm@ztwenqo.cn |
| Sender domain | ztwenqo.cn |
| Sending IP | 34.84.5.251 |
| Phishing URL | https://fumious.umfqrit.cn/login_index/ |
| Phishing domain | umfqrit.cn |
| Phishing domain IP | 104.21.83.54 |

---

## 7. Recommendation

- Block sender domain `ztwenqo.cn` and phishing domain `umfqrit.cn` at the email gateway / firewall level.
- Do not click any links or reply to this email.
- Report the email to Yodobashi's official abuse/security contact, as the brand is being actively impersonated.
- User awareness: train users to verify account-related emails by navigating directly to the official website rather than clicking embedded links.

---

## 8. Tools Used

- Kali Linux (isolated VM environment)
- `unzip`, `cat`, `grep` (Linux command line)
- Python 3 (`email` library for MIME/base64 decoding)
- VirusTotal (URL reputation scanning)

---

*This analysis was conducted using a real-world phishing sample for educational/portfolio purposes as part of cybersecurity skill-building.*
