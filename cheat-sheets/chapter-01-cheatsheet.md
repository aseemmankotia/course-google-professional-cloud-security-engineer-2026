# Chapter 1 Cheat Sheet: Cloud Identity, SSO & Workforce Identity Federation
**Exam Domain:** Configuring Access (25%)

## Cloud Identity Free vs Premium
| Feature | Free | Premium |
|---|---|---|
| User cap | 50 users | Unlimited |
| Endpoint management | Basic | Advanced (MDM, app mgmt) |
| Security Center/Vault-like features | No | Yes |
| SLA | No | Yes |

## Identity/Access Mechanism Comparison
| Mechanism | Who it's for | Account created in Cloud Identity? | Configured in |
|---|---|---|---|
| SAML SSO (3rd-party IdP) | Existing Cloud Identity/Workspace users | Yes — must pre-exist, SSO just federates auth | Admin console > Security > SSO with third-party IdP |
| Workforce Identity Federation | External human workforce (contractors, partners) | No | IAM > Workforce Identity Federation (pools + providers: SAML/OIDC) |
| Workload Identity Federation | External machine/service identities (AWS, on-prem, other clouds) | No | IAM > Workload Identity Federation |
| Directory Sync (GCDS) | Provisioning/deprovisioning human accounts from AD/LDAP | Yes — creates/updates/deletes users | On-prem tool, one-way AD → Cloud Identity |

**Golden rule:** If the scenario says *"no Google account should ever be created"* → Workforce Identity Federation (people) or Workload Identity Federation (machines). If it says *"employees should log in with existing corporate credentials but still have a Google identity"* → SAML SSO (requires the account to exist, via GCDS or manual provisioning).

## Super-Admin Hardening Checklist
- Dedicated super-admin account, never used for daily email/collaboration
- Enforce 2-Step Verification; prefer security-key-only enforcement
- Use FIDO/physical security keys (e.g., Titan Security Key)
- Enroll super-admins in Advanced Protection Program
- Super-admin role assignable to **individual users only**, never Groups

## Password & Session Policy Numbers
| Setting | Range/Value |
|---|---|
| Password length | 8–100 characters |
| Session length (Google session control) | 1 hour – 24 hours, or "never expire" |

## Directory Sync (GCDS) Notes
- Runs on-prem or on a VM with LDAP/AD connectivity — not a hosted GCP service
- One-way sync only: AD/LDAP → Cloud Identity (never back to AD)
- Syncs users, groups, non-editable custom attributes
- Command: `./sync-cmd -a -c config.xml -l logfile.log`

## Common Exam Traps
1. Confusing Workforce Identity Federation (people) with Workload Identity Federation (machines/services)
2. Assuming SAML SSO eliminates the need to provision Cloud Identity accounts — it does not
3. Assuming super-admin role can be delegated to a Google Group — it cannot
4. Mixing up Cloud Identity Free's 50-user cap with Workspace edition limits (they're separate)
