# Triage Report - ALERT-004
**Type:** Azure Privilege Escalation (Global Administrator Role Assignment)
**Source:** SIEM (Azure AD / Entra ID)
**Severity:** Critical
**Status:** Escalated to L2 / IR
**Analyst:** cyberweles
**Date:** 2025-12-10

---

## 1. Summary

A non-admin user account (`lukas.meier@example.com`) assigned **Global Administrator** rights to itself as a **permanent** role assignment in Azure AD / Entra ID.

The action was performed via the `RoleManagement.CreateRoleAssignment` operation using MS Graph API, outside the normal PIM workflow and without any justification.

Recent identity risk events (e.g. `unfamiliarSignInProperties`, `tokenAnomaly`) suggest that this may be the result of **account or token compromise**, leading to unauthorized privilege escalation with full tenant impact.

---

## 2. Key Indicators

| Indicator | Value |
| Actor (UPN) | `lukas.meier@example.com` |
| Actor admin status | Non-admin prior to event |
| Target of role assignment | `lukas.meier@example.com` (self-assignment) |
| Assigned role | Global Administrator |
| Assignment type | Permanent (not time-limited) |
| API method | `RoleManagement.CreateRoleAssignment` (MS Graph) |
| PIM usage | No eligible/active roles, no justification |
| Risk events | `unfamiliarSignInProperties`, `tokenAnomaly` |
| Recent auth activity | 5 failed logins, 1 success |

---

## 3. Evidence (log snippet)

Actor: lukas.meier@example.com
Action: RoleManagement.CreateRoleAssignment
Assigned role: Global Administrator (permament)
Justification: null
Method: MS Graph API
Role assignment outside PIM approval workflow
Related risk events: unfamiliarSignInProperties, tokenAnomaly

## 4. Analysis
Key points:
- The user was not an administrator before this event and should not normally be able to grant themselves highly privileged roles.
- The assigned role is Global Administrator, which provides full control over the tenant (users, apps, security settings, identities, etc.).
- The assignment is permanent, not a time-limited elevation through PIM, and was done without justification.
- The action used MS Graph API, which may indicate scripted or programmatic abuse rather than noemal portal-based admin work.
- Recent identity risk events (`unfamiliarSignInProperties`, `tokenAnomaly`) and multiple failed logins support the hypothesis of compromised credentials or abused refresh token.

Taken together, this strongly indicates unauthorized privilege escalation, with potential for full environment compromise, persistence creation, and large-scale impact.

Conclusion: True Positive with tenant-wide critical risk.

## 5. Assessment
- False Positive: No
- True Positive: Yes
- Confidance: High
- Severity: Critical

## 6. Decision (L1)
Immediate escalation to L2 / Incident Response.

This case cannot be closed at L1 and requires urgent containment and tenant-wide review.

## 7. Recommended Actions (for L2/IR)
- Immediately remove the Global Administrator role from `lukas.meier@example.com`.
- Force sign-out and reset credentials for the account (including MFA re-registration).
- Invalidate refresh tokens and app sessions associated with the user.
- Review all recent role assignments, app registrations, and consent grants performed by this account.
- Check for creation of additional privileged accounts, service principals or backdoor roles.
- Correlate with sign-in logs and risk events to understand how the account was compromised.
- Consider temporary restriction of admin activities and tighter PIM enforcement until environment is verified as clean.

## 8. Notes
This alert represents a high-impact cloud identify threat pattern: compromised user -> unauthorized Global Admin assignment -> potential tenant takeover.

Cases of this type should always be treated as critical and handled with full incident response procedures.

---