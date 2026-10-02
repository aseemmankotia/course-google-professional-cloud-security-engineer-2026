## Chapter 11: Shared Responsibility, Assured Workloads, and Compliance Scoping — Practice Questions

**Q1.** A federal contractor requires that Google support engineers must obtain explicit customer sign-off before viewing any customer content, with narrow emergency-only exceptions. Which Google Cloud capability satisfies this requirement?

A) Access Transparency
B) Access Approval
C) VPC Service Controls
D) Organization Policy Service

<details>
<summary>Answer</summary>

**Correct: B**

Access Approval requires Google to obtain explicit customer approval before accessing customer content, with an emergency override that admins can disable for stricter enforcement.

**Why the others are wrong:**
- Access Transparency only provides near-real-time logs of Google access after it happens — it does not gate access with an approval step.
- VPC Service Controls creates network/service perimeters around data resources; it has no relationship to Google support staff access approvals.
- Organization Policy Service constrains resource configuration (e.g., locations, service usage) — it does not control Google personnel access to customer content.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A US federal agency workload requires DoD Impact Level 5 controls. Which Assured Workloads control package should be selected?

A) FedRAMP High
B) CJIS
C) IL5
D) IRS 1075

<details>
<summary>Answer</summary>

**Correct: C**

IL5 (Impact Level 5) is the Assured Workloads control package purpose-built for DoD workloads requiring Impact Level 5 controls, including personnel and location restrictions.

**Why the others are wrong:**
- FedRAMP High is for civilian federal agencies handling high-impact data, not DoD IL5 requirements.
- CJIS applies to criminal justice information handled by law enforcement agencies, not DoD IL5 workloads.
- IRS 1075 applies to Federal Tax Information handling requirements, unrelated to DoD impact levels.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A Canadian financial institution must store and process data under Protected B controls with personnel restricted to Canada-based support. Which Assured Workloads control package is BEST?

A) EU Regions and Support
B) Australia Regions and Support
C) FedRAMP Moderate
D) Canada Regions and Support (Protected B)

<details>
<summary>Answer</summary>

**Correct: D**

Canada Regions and Support (Protected B) enforces Canadian data residency plus Canada-based support personnel restrictions matching Protected B requirements.

**Why the others are wrong:**
- EU Regions and Support restricts data/personnel to the EU, not Canada, and does not map to Protected B.
- Australia Regions and Support is scoped to Australian data residency, unrelated to Canadian Protected B.
- FedRAMP Moderate is a US federal civilian control package with no Canadian residency guarantees.

</details>

---

**Q4.** A company runs a fully managed service (BigQuery) and a self-managed application on Compute Engine. Under the shared responsibility model, which statement is accurate?

A) The customer is responsible for guest OS patching on Compute Engine, while Google manages the underlying infrastructure and OS for BigQuery
B) Google is responsible for guest OS patching on both Compute Engine and BigQuery
C) The customer is responsible for physical data center security in both cases
D) Google is never responsible for anything above the hypervisor layer regardless of service type

<details>
<summary>Answer</summary>

**Correct: A**

Compute Engine is IaaS — the customer owns guest OS, patching, and runtime; BigQuery is fully managed (SaaS-like) — Google owns the OS, runtime, and platform, leaving the customer responsible mainly for data and access controls.

**Why the others are wrong:**
- Google never manages guest OS patching for Compute Engine VMs — that remains customer responsibility.
- Physical data center security is always a Google responsibility across all service types, never the customer's.
- As services move toward fully managed/SaaS, Google's responsibility extends higher up the stack — this is the entire point of the shifting boundary.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** Compared to standard GKE, GKE Autopilot shifts more shared-responsibility burden to Google. Which responsibility moves from the customer to Google under Autopilot?

A) IAM role bindings for cluster access
B) Kubernetes Secrets content encryption keys chosen by the customer
C) Node OS patching, node provisioning, and node-level security configuration
D) Application-level code vulnerabilities

<details>
<summary>Answer</summary>

**Correct: C**

GKE Autopilot removes node management entirely — Google provisions, patches, and secures nodes, whereas in standard GKE the customer manages node pools and OS patching.

**Why the others are wrong:**
- IAM role bindings remain a customer responsibility in both standard GKE and Autopilot.
- Customer-managed encryption keys (CMEK) are still configured and owned by the customer if used, regardless of Autopilot.
- Application code vulnerabilities are always the customer's responsibility — Google cannot be responsible for code it doesn't write.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** An organization must prevent any Compute Engine resources from being created outside the EU. Which control should be applied?

A) Enable Access Approval at the organization level
B) Enable VPC Service Controls with an EU-only access level
C) Apply the constraints/gcp.resourceLocations organization policy restricted to EU location groups
D) Configure a Cloud Armor security policy restricting EU IP ranges

<details>
<summary>Answer</summary>

**Correct: C**

constraints/gcp.resourceLocations is the organization policy constraint that restricts where resources can be created, supporting values like in:eu-locations to enforce data residency.

**Why the others are wrong:**
- Access Approval controls Google personnel access to data, not where resources are physically provisioned.
- VPC Service Controls restricts data exfiltration across service perimeters; it does not constrain resource creation location.
- Cloud Armor filters network traffic by IP/geo at the edge — it has no effect on where GCP resources are provisioned.

</details>

---

**Q7.** Which Cloud Audit Logs type is enabled by default for all services, cannot be disabled, and is retained for 400 days?

A) Admin Activity audit logs
B) Data Access audit logs
C) Policy Denied audit logs
D) VPC Flow Logs

<details>
<summary>Answer</summary>

**Correct: A**

Admin Activity audit logs record configuration/metadata-changing API calls, are always on, cannot be disabled, and are retained for 400 days.

**Why the others are wrong:**
- Data Access audit logs are disabled by default for most services (except BigQuery), must be explicitly enabled, and default to 30-day retention.
- Policy Denied audit logs are on by default but can be disabled and default to 30-day retention, unlike Admin Activity's fixed 400-day retention.
- VPC Flow Logs are a networking telemetry feature, not part of Cloud Audit Logs at all.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** What is the core distinction between Access Transparency and Access Approval?

A) Access Transparency blocks Google access until approved; Access Approval only logs access after the fact
B) Access Transparency provides near-real-time logs of Google personnel access; Access Approval requires explicit customer authorization before that access occurs
C) They are two names for the identical feature with no functional difference
D) Access Transparency applies only to Compute Engine while Access Approval applies only to BigQuery

<details>
<summary>Answer</summary>

**Correct: B**

Access Transparency is a visibility/logging control (after-the-fact records of Google access with justification), while Access Approval is a preventive/gating control requiring explicit approval before access occurs.

**Why the others are wrong:**
- This reverses the two features' actual roles — Access Approval is the blocking/gating mechanism, not Access Transparency.
- They serve distinct, complementary purposes (visibility vs. control) and are configured separately.
- Both features apply broadly across supported Google Cloud services, not restricted to single services.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

