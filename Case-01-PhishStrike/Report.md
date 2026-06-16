# Phishing Analysis Report: [Case #1 PhishStrike]

## Case Metadata
- **Lab:** CyberDefenders - [PhishStrike Lab]
- **Date Analyzed:** 2026-06-15
- **Analyst:** [Abdulrhman-AKA:Levi]
- **Difficulty:** Easy
- **Time Spent:** 1 hour

---

## Executive Summary

A phishing email targeting faculty members at an educational institution was investigated. The email impersonated a trusted contact and contained a malicious link delivering multiple malware families including CoinMiner, BitRAT, and AsyncRAT. The attack was identified and IOCs were extracted to block malicious infrastructure and prevent further compromise.

---

## Email Overview
- **Subject Line:** "Commercial Purchase Receipt"
- **Sender (Display):** ERIKA JOHANA LOPEZ VALIENTE
- **Sender (Actual):** erikajohana.lopez@uptc.edu.co 
- **Recipient:** servicios.informaticos@fsfb.org.co
- **Date/Time:** Thu, 9 Dec 2022 14:58:42 UTC
- **Attachments:** None (malicious link in body))

---

## Header Analysis 
### Authentication Results 

-**SPF:** ❌ SOFTFAIL - Sender IP 18.208.22.104 not authorized for uptc.edu.co domain 

-**DKIM:** ❌ FAIL - No key for signature

-**DMARC:** ❌ NONE - No DMARC policy configured


### ### Email Routing (Bottom to Top)
1. Originating: mail-wr1-f65.google.com [209.85.221.65] 
2. Passed through: inpost.tmes.trendmicro.com [18.208.22.104] 
3. Delivered to: BL6PEPF0001AB51.mail.protection.outlook.com


### Key Findings 

- Sender IP: 18.208.22.104 (SPF softfail) 
- Return-Path: erikajohana.lopez@uptc.edu.co 
- DKIM failed 
- email integrity unverified 
- No DMARC policy 
- domain unprotected


## URL Analysis
- **URL:** http://107.175.247.199/loader/server.exe
- **Defanged:** http://107.175.247.199/loader/server.exe
- **Hosting IP:** 107.175.247.199
- **Purpose:** Malware delivery - multiple families
- **Verification:** URLHaus confirmed malicious

---

## Malware Analysis

### CoinMiner
- **Type:** Cryptocurrency Miner
- **URL Requested:** http://ripley.studio/loader/uploads/Qanjttrbv.jpeg
- **Impact:** Exploits system resources for crypto mining

### BitRAT
- **Persistence:** HKEY_CURRENT_USER\Software\Microsoft\
  Windows\CurrentVersion\Run
- **Executable:** Jzwwix.exe
- **SHA-256:** bf7628695c2df7a3020034a065397592a1f8850e59f9a448b555bc1c8c639539
- **Download URL:** hxxp://107.175.247.199/loader/server.exe
- **C2 Domain:** gh9st.mywire.org
- **Evasion:** 50-second PowerShell delay

### AsyncRAT
- **Exfiltration:** Telegram Bot
- **Telegram Bot ID:** bot5610920260

---

## MITRE ATT&CK Mapping
- **T1566.002** — Phishing: Spearphishing Link
- **T1496**    — Resource Hijacking (CoinMiner)
- **T1547.001** — Registry Run Keys (BitRAT Persistence)
- **T1071.001** — Web Protocols (C2 Communication)
- **T1041**    — Exfiltration via Telegram

---

## Indicators of Compromise (IOCs)

### Network IOCs
- IP: 18.208.22.104 (Sender)
- IP: 107.175.247.199 (Malware Server)
- Domain: gh9st.mywire.org (BitRAT C2)
- URL: hxxp://107.175.247.199/loader/server.exe
- URL: hxxp://ripley.studio/loader/uploads/Qanjttrbv.jpeg

### Email IOCs
- Sender: erikajohana.lopez@uptc.edu.co
- Return-Path: erikajohana.lopez@uptc.edu.co
- Subject: COMMERCIAL PURCHASE RECEIPT ONLINE 27 NOV

### File IOCs
- File: Jzwwix.exe
- SHA-256: bf7628695c2df7a3020034a065397592a1f8850e59f9a448b555bc1c8c639539
- Telegram Bot ID: bot5610920260

---

## Recommendations

### Immediate Actions
1. Block IP: 107.175.247.199 at Firewall
2. Block domain: gh9st.mywire.org at DNS
3. Search email logs for all recipients
4. Scan all endpoints for Jzwwix.exe
5. Report Telegram Bot ID: bot5610920260

### Long-Term Improvements
1. Implement DMARC enforcement policy
2. Deploy email link sandboxing
3. Conduct phishing awareness training
4. Monitor registry Run keys on endpoints

---

## Lessons Learned
- SPF softfail does not block email delivery
- No DMARC policy allows spoofing attacks
- Single phishing email can deliver multiple malware
- PowerShell delays used to evade sandbox detection
- Telegram bots increasingly used for C2 and exfiltration
