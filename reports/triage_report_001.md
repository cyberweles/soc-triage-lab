# Triage Report — ALERT-001

**Alert ID:** ALERT-001  
**Title:** Multiple failed SSH logins followed by a successful authentication  
**Source:** SIEM (Azure VM / Linux auth logs)  
**Analyst:** cyberweles  
**Date of analysis:** 2025-12-06  

---

## 1. Short Summary

A single external IP address (`203.0.113.45`) attempted approximately 120 failed SSH authentications against the `root` account on the Azure VM `webserver-01`. After these failures, the same IP successfully authenticated as root.  
This strongly suggests a brute-force attack that resulted in unauthorized access.

---

## 2. Key Indicators

- **Username:** root  
- **Source IP:** 203.0.113.45 (external)  
- **Destination host:** webserver-01.example.internal  
- **Event count:** ~120 failed attempts + 1 successful login  
- **Time window:** 12:15:03Z → 12:29:57Z  
- **Cloud provider:** Azure  

**Important log entries:**  
- Multiple `Failed password for root` events  
- Followed by `Accepted password for root`  
- Session opened (`uid=0`)  

**Additional notes:**  
- Root SSH login should not be used under normal circumstances.  
- Brute-force sequence followed by success indicates credential compromise.

---

## 3. Initial Assessment

- [ ] False Positive  
- [ ] Benign but risky  
- [x] True Positive — likely malicious  
- [ ] Needs more information  

**Reasoning:**  
- High number of failed authentication attempts from a single external IP.  
- Successful root login immediately after repeated failures (classic brute-force pattern).  
- No legitimate user behavior matches this pattern.  
- Root login over SSH is typically forbidden by policy.

---

## 4. Severity

- [ ] Low  
- [ ] Medium  
- [ ] High  
- [x] Critical  

**Justification:**  
- Compromise of the highest-privileged local account (`root`).  
- Server is publicly accessible (Azure VM).  
- Confirmed unauthorized access after brute force.  
- High risk of persistence, lateral movement or data tampering.

---

## 5. Decision (L1)

- [ ] Close as False Positive  
- [ ] Close as acceptable risk  
- [x] Escalate to L2 / IR  
- [ ] Create follow-up task  

**Actions / Recommendations:**  
- Immediately isolate the VM from the network (Azure network isolation).  
- Block the source IP on network security groups/firewall.  
- Disable SSH root login / enforce key-based authentication.  
- Reset credentials for all privileged accounts.  
- Collect additional logs (auth logs, bash history, process list, new users, cron jobs).

---

## 6. Notes for L2

Unauthorized root login observed on `webserver-01` following ~120 failed SSH attempts from `203.0.113.45`.  
Successful authentication and session creation confirm compromise.  
Recommend full incident response workflow: forensic log review, checking for persistence mechanisms, credential rotation, and VM isolation.

---

## 7. Personal Learning Notes (Optional)

- Brute-force → success = immediate escalation.  
- Root login should always raise severity.  
- Cloud-exposed VMs require strict SSH hardening.
