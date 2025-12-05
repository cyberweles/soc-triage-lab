# Triage Report — ALERT-001

**Alert ID:** ALERT-001  
**Title:** Multiple failed SSH logins followed by a successful authentication  
**Source:** SIEM (Azure VM / Linux auth logs)  
**Analyst:** cyberweles  
**Date of analysis:** 2025-01-24  

---

## 1. Short Summary
_Describe in 2–4 sentences what is happening, in your own words._

Example placeholder:  
A single external IP performed multiple failed SSH authentication attempts against `webserver-01`. After ~120 failures, a successful login for the `root` account was recorded. This may indicate a brute-force attack resulting in unauthorized access.

---

## 2. Key Indicators
- **Username:** root  
- **Source IP:** 203.0.113.45  
- **Destination host:** webserver-01.example.internal  
- **Event count:** 120 failed attempts + 1 successful login  
- **Time window:** 12:15:03Z → 12:29:57Z  
- **Cloud provider:** Azure  
- **Important log entries:**  
  - Failed password attempts  
  - Successful authentication  
  - Session opened  

_Add your additional notes here (GeoIP, prior events, policy expectations, etc.)._

---

## 3. Initial Assessment
Choose one:

- [ ] False Positive  
- [ ] Benign but risky  
- [ ] True Positive — likely malicious  
- [ ] Needs more information  

**Reasoning:**  
- …  
- …  

---

## 4. Severity
- [ ] Low  
- [ ] Medium  
- [ ] High  
- [ ] Critical  

**Justification:**  
- …  

---

## 5. Decision (L1)
- [ ] Close as False Positive  
- [ ] Close as acceptable risk  
- [ ] Escalate to L2 / IR  
- [ ] Create follow-up task  

**Actions / Recommendations:**  
- …  

---

## 6. Notes for L2
_What should L2 know? Be concise, technical and factual._  
- …  
- …  

---

## 7. Personal Learning Notes (Optional)
- …  
