# Triage Report - ALERT-002
**Type:** Impossible Travel (Azure AD)
**Source:** SIEM (Azure AD Sign-In Logs)
**Severity:** High
**Status:** Escalated to L2 / IR
**Analyst:** cyberweles
**Date:** 2025-12-07

---

## 1. Summary
Azure AD detected two successful sign-ins from the same user account (`alice.werner@example.com`) within a 12-minute window, originating from geographically impossible locations:

- Frankfurt, Germany
- New York, United States

The second sign-in used a different device and did **not** require MFA, which strongly indicates stolen credentials or session hijacking.

---

## 2. Key Indicators  
| Indicator | Value |
|----------|-------|
| User | `alice.werner@example.com` |
| First sign-in | DE → 198.51.100.23 (MFA required) |
| Second sign-in | US → 203.0.113.88 (MFA not required) |
| Time window | 08:12 → 08:24 UTC (12 minutes) |
| Estimated distance | ~6200 km |
| Device change | WIN10-LAPTOP-01 → Browser (Edge) |
| Risk flags | `impossibleTravel = true`, `risk_level = high` |

---

## 3. Evidence (log snippet)
Sign-in success: alice.werner@example.com from 198.51.100.23 (Frankfurt, DE), device WIN10-LAPTOP-01, MFA required.
Sign-in success: alice.werner@example.com from 203.0.113.88 (New York, US), device BROWSER-EDGE, MFA not required.
Risk event: Impossible travel detected (12 minutes between Germany and United States).

---

## 4. Analysis
The activity cannot be explained by normal user behavior.
Key findings:
- Physical travel between locations is impossible in the given time.
- MFA discrepancy (first sign-in MFA, second sign-in no MFA).
- Different devices for each sign-in.
- High-risk identity alert issued by Azure AD.

These indicators collectively support a **True Positive** identity compromise.

---

## 5. Assesment
- **False Positive:** No
- **True Positive:** Yes
- **Confidence:** High
- **Severity:** High

---

## 6. Decision (L1)
**Escalate to L2 / Incident Response.**

Reason: Strong evidence of credential theft or session hijack.

---

## 7. Recommended Action (for L2/IR)
- Contact user to confirm legitimacy of both sign-ins.
- Perform forces sign-out across all sessions.
- Reset password and re-enroll MFA.
- Review mailbox, file access, Teams/SharePoint activity.
- Check for other risky sign-ins or lateral movement.
- Identify any matching impossible-travel alerts on other accounts.

---

## 8. Notes
This case demonstrates a typical cloud identity compromise pattern.
Alert quality and context (risk-level + MFA mismatch) strongly support excalation.

---
