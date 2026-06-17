# Phishing Analysis Report: BRabbit Ransomware Campaign

## Case Metadata
- **Lab:** CyberDefenders - BRabbit
- **Date Analyzed:** 2026-06-17
- **Analyst:** Abdulrhman - AKA: Levi
- **Difficulty:** Medium
- **Time Spent:** 2 hours

---

## Executive Summary
A multi-vector phishing campaign was investigated targeting 
Drumbo company employees. The attack used a spoofed CEO email 
address to deliver Bad Rabbit ransomware via a malicious 
attachment. The ransomware established persistence through 
scheduled tasks and modified the system MBR, encrypting files 
and rendering the system unbootable. Attribution points to 
the Sandworm threat actor group.

---

## Email Overview
- **Subject Line:** Immediate Action Required: Your Employment Contract
- **Sender (Display):** Drumbo® (CEO James Smith)
- **Sender (Actual):** theceojamessmith@Drurnbo.com
- **Recipient:** Rafaiel@Drumbo.com
- **Date/Time:** Fri, 13 Dec 2024 22:27:38 +0000
- **Attachments:** Malicious executable (infpub.dat)

---

## Header Analysis

### Authentication Results
- **SPF:** ✅ PASS (spoofed domain SPF)
- **DKIM:** ⚠️ FAIL - Invalid DKIM signature
- **DMARC:** ❌ NOT COMPLIANT - No DMARC record found

### Email Routing (Bottom to Top)
1. Originating: efianalytics.com [216.244.76.116]
2. Relay: spruce-goose-ax.twitter.com [199.59.150.93]
3. Relay: dezoeteinval.co.za [51.195.254.223]
4. Delivered to: Rafaiel@Drumbo.com via Google MX

### Key Findings
- **Lookalike Domain:** theceojamessmith@Drurnbo.com
  (Drurnbo vs legitimate Drumbo)
- **Multiple IPs on Blacklist:** All relay hops flagged
- **SPF Spoofing:** Passes SPF but uses forged domain
- **No DMARC:** Domain unprotected against spoofing
- **Urgency Language:** "Immediate Action Required"
- **Authority Impersonation:** Fake CEO email

---

## Malicious Components

### Attachment
| Filename | File Type | Detection | Malware Family |
|----------|-----------|-----------|-----------------|
| infpub.dat | Executable | Malicious | Bad Rabbit |

**Attachment Analysis:**
- File drops ransomware payload
- Executes with system privileges
- Contains hardcoded credentials (user: alex)

---

## Attack Flow

1. **Initial Access:** Phishing email spoofing CEO
2. **Social Engineering:** Urgent employment contract claim
3. **User Action:** Employee opens infpub.dat attachment
4. **Execution:** Ransomware executes with elevated privileges
5. **Persistence:** Creates scheduled tasks (rhaegal, drogon)
6. **C2 Communication:** Connects to command server via HTTP (T1071.001)
7. **Encryption:** DiskCryptor driver modifies MBR
8. **Impact:** System rendered unbootable, files encrypted

---

## MITRE ATT&CK Mapping
- **T1566.001** — Phishing: Spearphishing Attachment
- **T1204.002** — User Execution: Malicious File
- **T1071.001** — Web Protocols (C2 Communication)
- **T1053.005** — Scheduled Task/Job (Persistence)
- **T1495** — Firmware Corruption (MBR modification)

---

## Indicators of Compromise (IOCs)

### Email IOCs

Sender: theceojamessmith@Drurnbo.com

Domain: dezoeteinval.co.za

Originating IP: 216.244.76.116 (efianalytics.com)

Relay IP: 51.195.254.223 (South Africa)

Subject: Immediate Action Required: Your Employment Contract

### File IOCs

Filename: infpub.dat

Hardcoded Username: alex

### Malware IOCs

Family: Bad Rabbit (Sandworm)

Persistence Tasks: rhaegal, drogon

Driver: DiskCryptor

Message: "Disable your anti-virus and anti-malware programs"

### C2 IOCs

Protocol: HTTP (T1071.001)

Method: Web-based command and control

---

## Recommendations

### Immediate Actions
1. Block sender domain: dezoeteinval.co.za at email gateway
2. Block originating IPs at firewall (216.244.76.116, 51.195.254.223)
3. Search email logs for all recipients of this email
4. Isolate infected systems from network immediately
5. Block scheduled tasks: rhaegal, drogon
6. Remove DiskCryptor driver from affected systems

### Long-Term Improvements
1. Implement DMARC enforcement policy (p=reject)
2. Deploy email attachment sandboxing (detonate in isolated VM)
3. Monitor for lookalike domain registrations
4. Conduct SOC training on CEO fraud indicators
5. Implement MFA for executive accounts
6. Enable MBR protection on all endpoints
7. Deploy EDR solution for behavioral monitoring

---

## Lessons Learned
- Lookalike domains (typosquatting) are highly effective
- CEO/authority impersonation bypasses technical controls
- Urgency language increases user click rates
- SPF passing does not prevent domain spoofing
- Scheduled task creation is easy persistence mechanism
- MBR modification requires physical access or kernel-level code
- Sandworm is known for destructive ransomware campaigns
- Email security + user training are critical defenses

---

## Threat Actor Attribution
**Attribution:** Sandworm (Russian state-sponsored APT)
**Known Tactics:**
- Multi-stage ransomware deployment
- MBR modification for system destruction
- Coordinated phishing campaigns
- Infrastructure spanning multiple countries

---
