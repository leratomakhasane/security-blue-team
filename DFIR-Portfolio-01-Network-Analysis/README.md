# Network Traffic Analysis – Unauthorized FTP Access Investigation

## Executive Summary

An attacker gained unauthorized access to a Windows 7 system using credentials exposed over unencrypted HTTP traffic, then accessed sensitive employee data via FTP. The attack required no exploitation—just protocol insecurity.

**Impact:** Confirmed data exposure, credential compromise  
**Root Cause:** Cleartext credential transmission over HTTP  
**Timeframe:** 3-minute attack window (04:48–04:51 AM)

---

## Scope

- **Platform:** Security Blue Team – Network Analysis Exercise
- **Tools Used:** Wireshark 4.x
- **Evidence Sources:** PCAP1.pcap, PCAP2.pcap
- **Investigation Type:** Network Traffic Analysis / Unauthorized Access Investigation

---

## Attack Timeline

| Time | Event | Actor |
|------|-------|-------|
| 04:48 AM | Victim transmits plaintext credentials over HTTP | VICTIM |
| 04:49 AM | Attacker captures password (`sbt123`) from HTTP stream | ATTACKER |
| 04:50 AM | Attacker authenticates to FTP service (port 8081) | ATTACKER |
| 04:51 AM | Attacker accesses `Employee_Information_CONFIDENTIAL.txt` | ATTACKER |

---

## Attack Narrative

### 1. Credential Exposure via HTTP

Administrative credentials were transmitted in plaintext over HTTP, allowing the attacker to capture them without exploitation.

**Evidence:**

![HTTP Credential Exposure](screenshots/02_http_credential_exposure.png)

The HTTP stream exposed the password `sbt123` in cleartext.

---

### 2. Unauthorized FTP Authentication

The attacker used the captured credentials to authenticate to an FTP service running on port 8081.

**Evidence:**

![FTP Login Success](screenshots/03_ftp_login_success.png)

The FTP server response `230 Login successful` confirms successful authentication.

---

### 3. Access to Sensitive Files

After authentication, the attacker retrieved malware and accessed confidential employee data.

**Evidence:**

![FTP File Retrieval](screenshots/05_ftp_file_retrieval.png)

Observed commands:
- `RETR malware.exe` – malicious executable downloaded
- `RETR Employee_Information_CONFIDENTIAL.txt` – sensitive file accessed

---

## Key Findings

| Finding | Severity |
|---------|----------|
| Cleartext credential exposure over HTTP | **HIGH** |
| Use of insecure FTP protocol | **HIGH** |
| Non-standard FTP port usage (8081) | MEDIUM |
| Access to confidential employee data | **HIGH** |

---

## Impact Assessment

- **Unauthorized Access:** Confirmed
- **Credential Compromise:** Confirmed
- **Data Exposure:** Confirmed (confidential employee information)
- **Attack Complexity:** Low

---

## Recommendations

1. **Enforce HTTPS** for all web applications transmitting credentials
2. **Replace FTP with SFTP/FTPS** to encrypt file transfers
3. **Implement network monitoring** to detect cleartext credential transmission
4. **Restrict access** to sensitive files using principle of least privilege
5. **Audit non-standard ports** for unauthorized services
6. **Deploy intrusion detection** to identify credential harvesting attempts

---

## Investigation Overview

### Protocol Hierarchy Analysis

![Protocol Overview](screenshots/01_protocol_overview.png)

Protocol analysis identified HTTP and FTP as the primary protocols involved in the attack chain.

---

## Full Investigation Report

For detailed analysis, evidence breakdown, methodology, and investigative reasoning:

➡️ **[View Full Report](FULL_REPORT.md)**

---

## Skills Demonstrated

- **Protocol-level evidence extraction** – Identified cleartext credential exposure in HTTP stream without relying on automated alerts
- **Attack timeline reconstruction** – Built temporal sequence from fragmented packet data across two PCAP files
- **Investigative pivoting** – Corrected initial protocol assumptions (TCP → UDP for SSDP) based on negative results
- **Evidence-driven reporting** – Structured findings around provable attacker actions, not tool outputs
- **Multi-protocol correlation** – Linked HTTP credential exposure to subsequent FTP authentication and file access

---

## Disclaimer

This investigation was conducted in a controlled lab environment as part of Security Blue Team training.

No real systems or networks were compromised.

---

## About This Investigation

This is Investigation 1 of 26 in my DFIR portfolio journey. Each investigation demonstrates hands-on network forensics, incident response, and threat hunting skills using industry-standard tools and methodologies.

**Connect:** [LinkedIn](https://linkedin.com/in/leratomakhasane) | [GitHub](https://github.com/leratomakhasane)
