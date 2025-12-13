# Triage Report - ALERT-005
**Type:** Suspicious OAuth App Content (High-Privilege Graph Scopes)
**Source:** SIEM (Azure AD / Entra ID)
**Severity:** High
**Status:** Escalated to L2 / IR
**Analyst:** cyberweles
**Date:** 2025-12-12

---

## 1. Summery
A non-admin user (`anna.kowalska@example.com`) granted OAuth consent to a **newly creadted, unverified application** ("Invoice Viewer").
The app received **high/privilege Microsoft Graph scopes** enabling access to mailbox and files.
No admin approval or business justification was present, indicating potential **persistent access via OAuth tokens**.

---

## 2. Key Indicators
| Indicator | Value |
|---|---|
| User | `anna.kowalska@example.com` (non-admin) |
| App | "Invoice Viewer" (unverified, newly created) |
| Consent type | User consent (browser) |
| Granted scopes | |`Mail.Read`, `Files.Read.All`, `User.Read.All` |
| Admin approval | Required but **not granted** |
| IP / Location | `185.212.44.91` / Unknown |
| Risk signals | Unverified publisher, high-priv scopes, new app |

---

## 3. Evidence (log snippet)
User anna.kowalska@example.com granted OAuth consent to app 'Invoice Viewer'
Granted scopes: Mail.Read, Files.Read.All, User.Read.All
Publisher verified: false
Admin approval: not granted
Graph API access token issued

## 4. Analysis
- OAuth ≠ login: consent can grant long-lived access without repeated authentication.
- The app is unverified and newly created, increasing risk of malicious intent.
- High-privilege Graph scopes enable broad data access (mailbox/files/users).
- No admin approval despite scopes typically requiring oversight.
- This pattern aligns with cloud persistence via OAuth abuse.

Conclusion: True Positive with high risk of data access and persistence.

## 5. Assessment
- False Positive: No
- True Positive: Yes
- Confidence: High
- Severity: High

## 6. Decision (L1)
Escalate to L2 / Incident Response.

## 7. Recommended Actions (for L2/IR)
- Revoke OAuth consent for the application and invalidate tokens.
- Review Graph activity performed by the app (mail/file access)
- Check for additional OAuth apps with similar scopes.
- Access user-consent policies; consider restricting user consent.
- Monitor for re-consent attempts or related identity anomalies.

## 8. Notes
This case demonstrates a common cloud persistence technique where OAuth consent bypasses traditional credential controls. Thight governance of app consent is critical.