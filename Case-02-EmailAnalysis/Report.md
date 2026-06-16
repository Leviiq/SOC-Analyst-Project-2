```
# Phishing Analysis Report: [Case 2]

## Case Metadata
- **Lab:** LetsDefend - Email Analysis
- **Date Analyzed:** 2026-06-16
- **Analyst:** Abdulrhman - AKA: Levi
- **Difficulty:** Medium
- **Time Spent:** 45 Minute

---

## Executive Summary
A phishing email impersonating United Scientific Equipment
was investigated. The email contained a malicious attachment
and used social engineering tactics around incorrect bank
details to trick the recipient. The attachment was confirmed
malicious via SHA256 hash lookup on VirusTotal.

---

## Email Overview
- **Subject Line:** united scientific equipment
- **Sender (Display):** Yan Ting (Ms)
- **Sender (Actual):** yanting@united.com.sg
- **Recipient:** admin@malware-traffic-analysis.net
- **Date/Time:** 08 Feb 2021 23:15:11 -0800
- **Attachments:** united_scientific_equipent.zip

---

## Header Analysis

### Authentication Results
- **SPF:** ❌ FAIL - IP 71.19.248.52 not authorized
  for united.com.sg domain
- **DKIM:** ❌ None - No signature found
- **DMARC:** ❌ Not compliant

### Email Routing (Bottom to Top)
1. Originating: united.com.sg [71.19.248.52]
2. Delivered to: malware-traffic-analysis.net

### Key Findings
- Sender IP: 71.19.248.52 is on a blacklist
- SPF authentication failed
- No DKIM signature found
- DMARC not compliant
- Email uses social engineering around bank details

---

## Malicious Components

### Attachment
| Filename | File Type | SHA256 | VT Detection |
|----------|-----------|--------|--------------|
| united_scientific_equipent.zip | ZIP Archive | 9909753bfb0ac8ab165bab3555233d03b01a9274a92e57c022f87ccbe51ca415 | Malicious |

---

## Attack Flow
1. **Initial Access:** Phishing email sent to recipient
2. **Social Engineering:** Email claims incorrect bank
   details on invoice to create urgency
3. **User Action:** Recipient opens malicious attachment
4. **Execution:** Malware executes on victim machine
5. **Post-Compromise:** Attacker gains access to system

---

## MITRE ATT&CK Mapping
- **T1566.001** — Phishing: Spearphishing Attachment
- **T1204.002** — User Execution: Malicious File
- **T1566.002** — Social Engineering via Email

---

## Indicators of Compromise (IOCs)

### Email IOCs
```

Sender: [yanting@united.com.sg](mailto:yanting@united.com.sg)  
Subject: united scientific equipment  
Sender IP: 71.19.248.52

```

### File IOCs
```

Filename: united_scientific_equipent.zip  
SHA256: 9909753bfb0ac8ab165bab3555233d03b01a9274a92e57c022f87ccbe51ca415

```

---

## Recommendations

### Immediate Actions
1. Block sender IP: 71.19.248.52 at email gateway
2. Search email logs for all recipients
3. Quarantine malicious attachment
4. Scan all endpoints that received this email

### Long-Term Improvements
1. Implement DMARC enforcement policy
2. Deploy email attachment sandboxing
3. Conduct user awareness training
4. Enable MFA for all accounts

---

## Lessons Learned
- SPF failure is a strong phishing indicator
- Social engineering around financial topics is common
- Always verify bank detail change requests via phone
- Malicious attachments can bypass email filters
