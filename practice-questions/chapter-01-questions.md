## Chapter 1: Cloud Identity, SSO, and Workforce Identity Federation — Practice Questions

**Q1.** A multinational corp has 5,000 contractors from partner companies who need temporary read access to BigQuery datasets but must never have a Cloud Identity or Workspace account created for them. Which solution meets this requirement?

A) Configure Workforce Identity Federation with their existing corporate IdP
B) Create Cloud Identity Free accounts for each contractor
C) Set up Google Cloud Directory Sync from the partner's Active Directory
D) Use Cloud Identity Premium with third-party SAML SSO

<details>
<summary>Answer</summary>

**Correct: A**

Workforce Identity Federation grants IAM access to external identities using their own SAML/OIDC IdP with no Cloud Identity/Workspace account ever created.

**Why the others are wrong:**
- Creating Cloud Identity Free accounts directly violates the 'no account' requirement
- GCDS provisions actual Cloud Identity user objects, which still means accounts are created
- SAML SSO with Cloud Identity requires an existing Cloud Identity user object mapped by email; accounts still exist

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** Security policy requires that if a super-admin's laptop is compromised while doing routine work, the attacker cannot pivot into Workspace admin functions. What is the BEST practice?

A) Create a dedicated super-admin account not used for daily email/collaboration, protected with security keys and mandatory 2-Step Verification
B) Enable password expiration for all users
C) Assign the super-admin role to a Google Group for shared visibility
D) Enforce OAuth app allowlisting only

<details>
<summary>Answer</summary>

**Correct: A**

A dedicated, hardware-key-protected super-admin account isolates admin privileges from the routine identity that is most exposed to phishing/malware.

**Why the others are wrong:**
- Password expiration doesn't address account isolation
- Admin roles, including super-admin, cannot be assigned to a Google Group at all
- OAuth allowlisting is a separate control and doesn't isolate the super-admin identity

</details>

---

**Q3.** A newly acquired subsidiary has 20,000 employees in on-prem Active Directory using Okta as their IdP. Security requires: (1) employees automatically provisioned/deprovisioned in Cloud Identity as AD changes, and (2) employees authenticate with their existing Okta credentials into Google Workspace. Which solution satisfies BOTH requirements?

A) Configure SAML SSO with Okta only; SSO will handle account creation automatically
B) Use Workforce Identity Federation with Okta as the workforce pool provider
C) Deploy Google Cloud Directory Sync (GCDS) to provision/deprovision Cloud Identity users from AD, and configure a third-party SSO profile with Okta as the SAML IdP
D) Manually create each user account in the Admin console and enable 2-Step Verification

<details>
<summary>Answer</summary>

**Correct: C**

GCDS handles automated provisioning/deprovisioning from AD, while a SAML SSO profile handles the authentication hand-off to Okta — together they satisfy both requirements.

**Why the others are wrong:**
- SAML SSO never provisions accounts; the Cloud Identity user object must already exist for SSO to map an identity
- Workforce Identity Federation is designed to avoid creating Cloud Identity accounts, contradicting the provisioning requirement
- Manual account creation doesn't scale and provides no automated lifecycle sync

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** Which Google Cloud Identity edition is required to enforce security-key-only 2-Step Verification, use advanced endpoint (mobile device) management, and have no cap on the number of users?

A) Cloud Identity Premium
B) Cloud Identity Free
C) Google Workspace Business Starter
D) Google Workspace Essentials

<details>
<summary>Answer</summary>

**Correct: A**

Cloud Identity Premium removes the 50-user cap and unlocks advanced endpoint management and enhanced security controls.

**Why the others are wrong:**
- Cloud Identity Free is capped at 50 users and lacks advanced endpoint management
- Business Starter is a Workspace productivity-app edition, not the Cloud Identity tier being asked about
- Essentials is also a productivity-app edition, not a Cloud Identity edition distinction

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** A team runs workloads in AWS that need to call Google Cloud APIs using AWS IAM role credentials, without any Google identity being created. Which feature should they use?

A) Workload Identity Federation
B) Workforce Identity Federation
C) Google Cloud Directory Sync
D) Downloaded service account keys

<details>
<summary>Answer</summary>

**Correct: A**

Workload Identity Federation lets external machine identities (like AWS IAM roles) exchange credentials for short-lived GCP access tokens without service account keys.

**Why the others are wrong:**
- Workforce Identity Federation is for human/workforce users, not machine workloads
- GCDS provisions human user accounts, unrelated to workload credential federation
- Downloading and using long-lived SA keys is a security anti-pattern the exam flags as wrong

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** Where does an admin configure automatic expiration of all Google Workspace web sessions after a set duration (up to 24 hours)?

A) Admin console > Security > API controls
B) Admin console > Security > Google session control
C) Admin console > Security > Password management
D) IAM > Organization Policies

<details>
<summary>Answer</summary>

**Correct: B**

Google session control under Security settings lets admins set web session length, forcing periodic re-authentication.

**Why the others are wrong:**
- API controls manage OAuth app access to APIs, not session duration
- Password management governs password length/reuse/expiration, not session timeout
- Organization Policies are IAM resource constraints, unrelated to Workspace session length

</details>

---

**Q7.** What is the valid password length range an administrator can enforce in Cloud Identity/Workspace password management settings?

A) 6 to 30 characters
B) 8 to 100 characters
C) 4 to 64 characters
D) 10 to 128 characters

<details>
<summary>Answer</summary>

**Correct: B**

The Admin console password management policy allows enforcing a minimum/maximum length between 8 and 100 characters.

**Why the others are wrong:**
- Range too narrow/incorrect
- Not the actual enforceable range
- Not the actual enforceable range

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** What is the fundamental difference between configuring third-party IdP SAML SSO for Cloud Identity users and using Workforce Identity Federation?

A) SAML SSO is only for admins, Workforce Identity Federation is only for regular users
B) Workforce Identity Federation only supports OIDC while SAML SSO only supports SAML
C) SAML SSO requires an existing Cloud Identity user object mapped to the external identity; Workforce Identity Federation grants IAM access to external identities without any Cloud Identity/Workspace account existing
D) There is no real difference; both are configured in the same Admin console section

<details>
<summary>Answer</summary>

**Correct: C**

This is the core exam distinction: SAML SSO federates authentication to an existing Cloud Identity account, while Workforce Identity Federation avoids creating any Google account at all, granting IAM permissions directly to the external principal.

**Why the others are wrong:**
- False restriction — neither is admin/user-role-specific by design
- Workforce Identity Federation supports both SAML 2.0 and OIDC providers
- Workforce Identity Federation is configured in IAM (Workforce Pools), not the Admin console SSO profile section

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

