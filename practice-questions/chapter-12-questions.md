## Chapter 12: Full Practice Exam Simulation and 120-Minute Time-Management Strategy — Practice Questions

**Q1.** A company needs to grant an on-call engineer temporary elevated access to a Compute Engine project only between 2am and 4am UTC, with automatic expiration and no manual cleanup step. Which solution BEST meets this requirement?

A) Create a custom role and permanently grant it to the engineer's account
B) Generate short-lived OAuth tokens manually each night before the maintenance window
C) Add the engineer to a Google Group with the role and remove them manually afterward
D) Use an IAM Condition with a time-based expression on the role binding

<details>
<summary>Answer</summary>

**Correct: D**

IAM Conditions support date/time-based expressions directly on a role binding, so access is scoped to the exact window without any manual enforcement step.

**Why the others are wrong:**
- Permanent grant violates least privilege and never expires
- Manual token generation is error-prone and not an enforced control
- Group membership still requires a human to remember to remove access, defeating the auto-expiry requirement

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** An organization wants to prevent VM instances from exfiltrating data to unauthorized Google Cloud projects while still allowing private access to Google APIs. Which solution BEST meets this requirement?

A) Configure Cloud NAT on all subnets
B) Create firewall rules denying egress to 0.0.0.0/0
C) Enable Private Google Access on the subnet
D) Implement VPC Service Controls with a service perimeter around approved projects

<details>
<summary>Answer</summary>

**Correct: D**

VPC Service Controls create a perimeter that blocks data movement to services/projects outside the perimeter, even over Google's private network with valid credentials.

**Why the others are wrong:**
- Cloud NAT enables outbound internet access, it doesn't restrict which GCP projects can be reached
- Firewall rules operate at the network layer and don't control API-level data movement between Google-managed services
- Private Google Access enables private API connectivity but does not restrict which destination projects/services are reachable

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A healthcare company must control the rotation schedule of encryption keys protecting BigQuery datasets and be able to instantly render the data unreadable in an emergency. Which solution BEST meets this requirement?

A) Google-managed encryption keys (default)
B) Customer-managed encryption keys (CMEK) via Cloud KMS
C) Customer-supplied encryption keys (CSEK)
D) Application-layer encryption before upload

<details>
<summary>Answer</summary>

**Correct: B**

CMEK via Cloud KMS gives the customer control over key rotation schedule and lets them disable or destroy the key to instantly render data unreadable, and it integrates natively with BigQuery.

**Why the others are wrong:**
- Default Google-managed keys give the customer no rotation or revocation control
- BigQuery does not support CSEK at all
- Application-layer encryption bypasses BigQuery's native query features and adds unnecessary complexity

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** Which service should be used to store and version application secrets such as API keys and database passwords, with fine-grained IAM access, without managing the underlying encryption keys directly?

A) Cloud KMS
B) Secret Manager
C) Cloud Storage with CMEK
D) Config Connector

<details>
<summary>Answer</summary>

**Correct: B**

Secret Manager is purpose-built for storing, versioning, and controlling IAM access to secret values.

**Why the others are wrong:**
- Cloud KMS manages cryptographic keys, not secret values themselves
- Cloud Storage with CMEK requires manual versioning and access management, it is not purpose-built for secrets
- Config Connector manages GCP resources declaratively via Kubernetes and is unrelated to secret storage

</details>

---

**Q5.** Which tool provides centralized, near real-time detection of misconfigurations, vulnerabilities, and threats across Compute Engine, App Engine, and GKE within an organization?

A) Cloud Monitoring
B) Security Command Center
C) Cloud Logging
D) Access Transparency

<details>
<summary>Answer</summary>

**Correct: B**

Security Command Center aggregates security findings, vulnerabilities, and threats across assets into a centralized dashboard.

**Why the others are wrong:**
- Cloud Monitoring tracks metrics and uptime, not security posture
- Cloud Logging stores raw logs but doesn't organize them into security findings on its own
- Access Transparency logs Google staff access to customer data, unrelated to misconfiguration scanning

</details>

---

**Q6.** An auditor needs Cloud Audit Logs to capture read access to Cloud Storage objects containing regulated data, but these reads are currently not being logged. What must be configured?

A) Enable Admin Activity logs
B) Enable VPC Flow Logs
C) Enable Access Transparency
D) Enable Data Access audit logs for the Cloud Storage service

<details>
<summary>Answer</summary>

**Correct: D**

Data Access logs (DATA_READ) record read operations on customer data and must be explicitly enabled per service (except BigQuery, which is on by default).

**Why the others are wrong:**
- Admin Activity logs are always on but only capture configuration changes, not data reads
- VPC Flow Logs capture network traffic metadata, not object-level read events
- Access Transparency logs Google staff access, not customer access to their own data

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** An organization wants to guarantee that no Compute Engine instance in any project across the organization can ever be created with an external IP address, without relying on per-project firewall rules. What should they configure?

A) An Organization Policy constraint (constraints/compute.vmExternalIpAccess) set to deny
B) A firewall rule in each project denying ingress from 0.0.0.0/0
C) A Cloud Armor security policy
D) Cloud NAT applied to every VPC

<details>
<summary>Answer</summary>

**Correct: A**

Organization Policy constraints enforce guardrails hierarchically at org/folder/project level, preventing external IP assignment regardless of any project's firewall configuration.

**Why the others are wrong:**
- Firewall rules control traffic, not whether an external IP can be assigned, and must be repeated per project
- Cloud Armor protects HTTP(S) load balancer backends and is unrelated to VM IP assignment
- Cloud NAT enables outbound access without an external IP but does not prevent one from being assigned

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** What is the PRIMARY difference between a predefined IAM role and a custom IAM role in Google Cloud?

A) Predefined roles can be applied only at the project level, custom roles at any level
B) Predefined roles cannot be granted to service accounts
C) Custom roles are only usable within Kubernetes Engine
D) Custom roles let you bundle a specific set of permissions, while predefined roles are curated and maintained by Google with fixed permission sets

<details>
<summary>Answer</summary>

**Correct: D**

This is the defining distinction: predefined roles are Google-curated with fixed permissions, custom roles let admins assemble a precise permission set.

**Why the others are wrong:**
- Both role types can be applied at org, folder, or project level
- Predefined roles can absolutely be granted to service accounts
- Custom roles are usable across most GCP resources, not limited to GKE

</details>

---

