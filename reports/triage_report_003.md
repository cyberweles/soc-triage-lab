# Triage Report - ALERT-003
**Type:** Suspicious PowerShell Execution (Encoded Command)
**Source:** EDR (Microsoft Defender for Endpoint)
**Severity:** High
**Status:** Escalated to L2 / IR
**Analyst:** cyberweles
**Date:** 2025-12-08

---

## 1. Summary
The EDR detected PowerShell executed with stealth parameters and a Base64-encoded payload.
The command was launched by a non-admin user (`marcel.schmidt`) and immediately attempted an outbound HTTPS connection to a suspicious domain.

The combination of encoded command, obfuscation flags, and external network activity strongly suggests malicious script execution.

---

## 2. Key Indicators
| Indicator | Value |
|----------|--------|
| User | `marcel.schmidt` (non-admin) |
| Parent process | `explorer.exe` |
| Executed process | `powershell.exe` |
| Command line | `-NoP -NonI -W Hidden -EncodedCommand ...` |
| Encoded payload | Yes (Base64) |
| Outbound connection | `185.199.110.153:443` |
| Domain | `malicious-test-payload.example` |
| Risk signals | Encoded PowerShell, Stealth flags, Suspicious domain |

---

## 3. Evidence (log snippet)
PowerShell launched with suspicious flags (-NoP, -EncodedCommand)
Base64 payload detected
Outbound HTTPS connection to 185.199.110.153
Detection: `SuspiciousPowerShellCommand`

---

## 4. Analysis  
Key findings supporting malicious activity:

- PowerShell execution used stealthy flags typically seen in malware delivery:
  - `-NoProfile`
  - `-NonInteractive`
  - `-WindowStyle Hidden`
  - `-EncodedCommand`
- Base64-encoded payload indicates obfuscation.  
- User is non-admin and unlikely to run advanced PowerShell commands.  
- Outbound connection to a suspicious domain immediately after execution.  
- Parent process is `explorer.exe` → often seen when users unknowingly execute malicious attachments.

This pattern matches common techniques used in phishing → initial payload execution → command-and-control staging.

Conclusion: **True Positive**, likely part of initial access or malware staging.

---

## 5. Assessment  
- **False Positive:** No  
- **True Positive:** Yes  
- **Confidence:** High  
- **Severity:** High  

---

## 6. Decision (L1)  
**Escalate to L2 / Incident Response.**

The encoded payload and network activity indicate potential compromise requiring deeper investigation.

---

## 7. Recommended Actions (for L2/IR)  
- Retrieve full decoded PowerShell payload.  
- Check for persistence mechanisms (registry, scheduled tasks, startup folder).  
- Review recent file execution and download history.  
- Isolate the endpoint if additional signs of compromise appear.  
- Block the domain and IP at firewall/proxy level.  
- Evaluate lateral movement attempts and credential theft indicators.

---

## 8. Notes  
Encoded PowerShell with outbound C2-like traffic is a high-fidelity detection.  
This case should be prioritized for deeper forensic review.

---