## Chapter 2: Service Accounts, Workload Identity Federation, and Impersonation — Practice Questions

**Q1.** A company runs a GitHub Actions pipeline that must deploy artifacts to Google Cloud Storage. Security policy prohibits storing any long-lived credentials in the CI/CD system. Which solution BEST meets this requirement?

A) Create a service account, generate a JSON key, and store it as an encrypted GitHub secret
B) Create a service account and grant the GitHub Actions runner's IP address access via VPC Service Controls
C) Configure Workload Identity Federation with a GitHub OIDC provider mapped to a service account with storage permissions
D) Use domain-wide delegation to let the pipeline impersonate a Workspace admin user

<details>
<summary>Answer</summary>

**Correct: C**

Workload Identity Federation lets GitHub's OIDC tokens be exchanged for short-lived GCP credentials with no key material ever created or stored.

**Why the others are wrong:**
- A JSON key is a long-lived credential — exactly what the policy forbids, even if encrypted
- IP-based access doesn't authenticate identity and doesn't grant IAM permissions to call GCS APIs
- Domain-wide delegation is for Workspace user impersonation, unrelated to CI/CD-to-GCS deployment and far broader than needed

</details>

---

**Q2.** An enterprise runs workloads on AWS EC2 instances that need to write to a GCP BigQuery dataset. No GCP service account keys may leave AWS. Which configuration satisfies this?

A) Export a service account key, encrypt it with AWS KMS, and store it in Secrets Manager
B) Grant the AWS account's root user the BigQuery Data Editor role directly
C) Create a workload identity pool with an AWS provider, mapping the EC2 instance role ARN to a GCP service account via attribute conditions
D) Use VPC peering between AWS and GCP so the instance authenticates via internal IP

<details>
<summary>Answer</summary>

**Correct: C**

A workload identity pool with an AWS provider lets AWS IAM role credentials (already present via the instance metadata service) be exchanged for short-lived GCP tokens without ever creating a key.

**Why the others are wrong:**
- Storing a key anywhere, even encrypted, is still a long-lived credential leaving AWS and violates the requirement
- IAM does not accept AWS account/root ARNs directly for API-level BigQuery access without WIF as the trust bridge
- VPC peering provides network connectivity, not identity/authentication — GCP still requires an IAM principal

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A security team wants to enforce, organization-wide, that no new service account keys can ever be created in any project. Which control should they apply?

A) Remove roles/iam.serviceAccountKeyAdmin from all users
B) Set the organization policy constraint iam.disableServiceAccountKeyCreation to enforced at the org node
C) Enable VPC Service Controls with a service perimeter around IAM
D) Configure a Cloud Function trigger that deletes any key within 24 hours of creation

<details>
<summary>Answer</summary>

**Correct: B**

The iam.disableServiceAccountKeyCreation organization policy constraint, enforced at the org node, blocks key creation across all projects regardless of IAM roles held.

**Why the others are wrong:**
- Removing a role only affects that role; other roles/permissions (owner, custom roles) could still create keys
- VPC Service Controls restricts API access from outside a perimeter, it does not disable a specific IAM action org-wide
- A reactive delete-after-creation function still allows a key to exist and potentially be exfiltrated in the interim

</details>

---

**Q4.** A developer needs to run a script locally that authenticates as a service account without ever downloading a key file. Which IAM role must be granted to the developer on that service account?

A) roles/iam.serviceAccountUser
B) roles/iam.serviceAccountKeyAdmin
C) roles/iam.serviceAccountTokenCreator
D) roles/iam.serviceAccountAdmin

<details>
<summary>Answer</summary>

**Correct: C**

roles/iam.serviceAccountTokenCreator grants generateAccessToken/generateIdToken permissions, enabling impersonation (short-lived credentials) via gcloud --impersonate-service-account or the Security Token Service.

**Why the others are wrong:**
- serviceAccountUser lets you attach the SA to a resource (actAs) but does not grant impersonation token generation
- serviceAccountKeyAdmin manages keys — the opposite of the keyless goal here
- serviceAccountAdmin can manage the SA's configuration but is broader than needed and doesn't itself grant token generation by default

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** Which service is the correct choice to allow an on-premises Active Directory-federated application to obtain short-lived GCP credentials without a service account key?

A) Cloud Identity-Aware Proxy
B) Workload Identity Federation with a generic OIDC/SAML provider
C) Cloud VPN with IAM-authenticated tunnels
D) Domain-wide delegation configured in the Admin console

<details>
<summary>Answer</summary>

**Correct: B**

Workload Identity Federation supports generic OIDC or SAML providers, which covers on-prem AD FS/ADFS-issued tokens, exchanging them for short-lived GCP access tokens.

**Why the others are wrong:**
- IAP protects HTTP(S) access to apps behind a load balancer; it doesn't federate workload identities for API calls
- Cloud VPN provides network-layer connectivity, not an identity federation/authentication mechanism
- Domain-wide delegation is scoped to Google Workspace user impersonation, not on-prem AD federation

</details>

---

**Q6.** A security engineer configures a Workload Identity Federation provider for a GitHub Actions OIDC token. To prevent any repository other than 'org/prod-deploy' from assuming the mapped service account, what should be configured?

A) A firewall rule restricting GitHub's IP ranges
B) An attribute condition on the provider restricting assertion.repository to 'org/prod-deploy'
C) A separate project for each GitHub repository
D) An IAM deny policy blocking all other service accounts

<details>
<summary>Answer</summary>

**Correct: B**

Attribute conditions (CEL expressions) evaluate claims in the OIDC token, such as assertion.repository, restricting which external identities can complete the exchange — this is the exam-tested mechanism for scoping WIF trust.

**Why the others are wrong:**
- GitHub-hosted runners use dynamic IPs, making IP-based restriction impractical and not the intended control
- Splitting into separate projects is overhead and doesn't address the actual trust boundary at the provider level
- A deny policy on unrelated service accounts does nothing to restrict which repository can use this specific mapping

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** A newly created Compute Engine instance is found to have the Editor role via its attached default service account. What is the recommended remediation aligned with least privilege?

A) Leave the default service account as-is since Compute Engine requires Editor to function
B) Delete the default service account entirely from the project
C) Grant the instance the Owner role instead to simplify troubleshooting
D) Attach a custom service account with only the specific roles needed and remove Editor from the default SA, or set the org policy to disable automatic Editor grants

<details>
<summary>Answer</summary>

**Correct: D**

Best practice is to create purpose-built service accounts scoped to the workload's exact needs and either remove Editor from the default SA or use iam.automaticIamGrantsForDefaultServiceAccounts to stop the broad grant on new projects.

**Why the others are wrong:**
- Editor is a legacy default grant for convenience, not a functional requirement — Compute Engine runs fine with minimal roles
- Deleting the default SA can break existing workloads still relying on it and doesn't address the least-privilege pattern for future instances
- Owner is even broader than Editor and moves further from least privilege, not closer

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** What is the primary security risk of enabling domain-wide delegation on a service account?

A) It allows the service account to impersonate ANY user in the Workspace domain without per-user consent, effectively bypassing per-resource access controls
B) It causes increased Cloud Billing charges for API calls
C) It requires storing a JSON key, which is otherwise avoidable
D) It disables Cloud Audit Logs for that service account

<details>
<summary>Answer</summary>

**Correct: A**

Domain-wide delegation grants a service account the ability to act as any user in the Workspace domain for the scopes configured, making a single compromised key or credential equivalent to full domain user impersonation — a major blast-radius risk.

**Why the others are wrong:**
- Billing impact is incidental, not the core security concern being tested
- Domain-wide delegation historically often does use a key, but the risk being tested is the impersonation scope, not credential type alone
- Audit logging is unaffected by delegation configuration

</details>

---

