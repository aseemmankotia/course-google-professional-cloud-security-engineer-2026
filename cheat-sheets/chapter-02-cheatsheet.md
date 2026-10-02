## Chapter 2 Cheat Sheet: Service Accounts, WIF, Impersonation

### Domain: Configuring Access (25%)

### Keys vs WIF vs Impersonation
| Method | Lifetime | Key file? | Exam preference |
|---|---|---|---|
| Service account key | Long-lived (no auto-expiry) | Yes (JSON) | Avoid — use only when no alternative exists |
| Workload Identity Federation | Short-lived token exchange | No | Preferred for external workloads (AWS, on-prem, CI/CD) |
| Impersonation (SA-to-SA / user-to-SA) | 1hr default, up to 12hr | No | Preferred for human/internal access |

### Key IAM Roles
| Role | Grants | Use case |
|---|---|---|
| roles/iam.serviceAccountUser | actAs the SA on a resource | Attach SA to Compute/Cloud Run/Functions |
| roles/iam.serviceAccountTokenCreator | generateAccessToken/generateIdToken/signBlob/signJwt | Impersonation, short-lived credentials |
| roles/iam.workloadIdentityUser | Bind external/K8s identity to SA | GKE Workload Identity, WIF |
| roles/iam.serviceAccountKeyAdmin | Manage keys (create/delete) | Should be tightly restricted |
| roles/iam.serviceAccountAdmin | Full SA management | Broad — avoid granting widely |

### Org Policy Constraints
| Constraint | Effect |
|---|---|
| iam.disableServiceAccountKeyCreation | Blocks new Google-managed key creation |
| iam.disableServiceAccountKeyUpload | Blocks uploading externally-generated public keys |
| iam.automaticIamGrantsForDefaultServiceAccounts | Stops auto Editor-role grant to default SAs on new projects |

### WIF Setup Flow
1. Create pool: `gcloud iam workload-identity-pools create POOL --location=global`
2. Create provider: `create-aws` / `create-oidc` / `create-saml`
3. Add attribute mapping (map external claims → google.subject, attribute.*)
4. Add attribute condition (CEL) to restrict which external identities qualify (e.g., specific repo, AWS role ARN)
5. Grant roles/iam.workloadIdentityUser on the target SA to the external principal set
6. External system exchanges its native token (AWS STS, GitHub OIDC, SAML assertion) for a short-lived GCP token — no key ever created

### Numbers to Remember
- Max 10 user-managed keys per service account
- Default impersonation token lifetime: 1 hour (3600s), max 12 hours (43200s)
- Default Compute Engine/App Engine SA historically = Editor role (broad — remediate)

### Gotchas
- Cross-project SA use requires the grant on the SA resource itself, not just project-level IAM in the consumer project
- serviceAccountUser ≠ impersonation — it only allows attaching/actAs, not generating tokens
- Domain-wide delegation is configured in Workspace Admin console, not IAM — and impersonates ANY domain user, a major blast-radius risk
- disableServiceAccountKeyCreation vs disableServiceAccountKeyUpload are separate constraints testing separate scenarios (create vs. bring-your-own key)
