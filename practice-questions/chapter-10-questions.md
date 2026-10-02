## Chapter 10: Cloud Audit Logs, Cloud IDS, and Security Command Center — Practice Questions

**Q1.** A security engineer needs to capture every read of objects in a Cloud Storage bucket for compliance, in addition to the logs Google already writes by default. What must they do?

A) Nothing — read access is already logged in Admin Activity logs by default
B) Create a log sink to export System Event logs to BigQuery
C) Enable VPC Flow Logs on the subnet hosting the bucket's regional endpoint
D) Enable Data Access audit logs for the Cloud Storage service in the IAM Audit Logs page

<details>
<summary>Answer</summary>

**Correct: D**

Data Access logs record reads/writes of user data (e.g., GET/READ on objects) and are opt-in for almost every service except BigQuery, which has Data Access logs enabled by default.

**Why the others are wrong:**
- Admin Activity logs only record configuration/metadata changes (e.g., bucket creation, IAM changes), not object data reads.
- System Event logs are Google-generated infrastructure actions, not user data access, and are unrelated to object reads.
- VPC Flow Logs capture IP-layer network flow metadata, not application-layer object access, and Cloud Storage doesn't sit on a VPC subnet.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** An organization wants a single, org-wide export of all Cloud Audit Logs from every current and future project into one BigQuery dataset for a SIEM, without having to configure a sink in each project individually. What should they configure?

A) An aggregated sink at the organization level with includeChildren set to true
B) A log sink in the organization's billing project with --include-children
C) A separate log sink in each project pointing to the same BigQuery dataset
D) A Cloud Monitoring workspace scoped to the organization

<details>
<summary>Answer</summary>

**Correct: A**

Aggregated sinks are created at the organization or folder level and, with includeChildren enabled, automatically capture matching logs from all folders and projects underneath — including projects created afterward — without per-project configuration.

**Why the others are wrong:**
- A billing project has no special log-aggregation capability; sinks are scoped to the resource they're created on, not implicitly org-wide.
- Per-project sinks work but require manual configuration in every project and don't automatically cover future projects, failing the stated requirement.
- Cloud Monitoring workspaces aggregate metrics/dashboards, not Cloud Audit Log export to BigQuery.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A company wants to inspect east-west traffic between VMs in a VPC for known malware signatures without deploying agents on each VM or routing traffic through a third-party appliance. Which service should they deploy?

A) VPC Flow Logs with a Log Analytics query for suspicious IPs
B) Cloud IDS with a Packet Mirroring policy on the relevant subnet
C) Cloud Armor with a custom WAF rule
D) Cloud NAT with logging enabled

<details>
<summary>Answer</summary>

**Correct: B**

Cloud IDS uses Palo Alto Networks signature-based threat detection on mirrored, out-of-band copies of traffic (via an automatically created Packet Mirroring policy), giving deep packet inspection without agents or inline appliances.

**Why the others are wrong:**
- VPC Flow Logs only capture flow metadata (5-tuple, bytes, packets) — no payload inspection or signature matching for malware.
- Cloud Armor protects HTTP(S) load-balanced ingress traffic at Layer 7, not general east-west VM-to-VM traffic.
- Cloud NAT logging only records outbound NAT connection events, not payload-level threat signatures.

</details>

---

**Q4.** A finding from Event Threat Detection appears in the Security Command Center console for one project, but the security team cannot find any Container Threat Detection or Web Security Scanner findings across the organization. What is the most likely cause?

A) Event Threat Detection is misconfigured and needs a separate API enablement
B) Container Threat Detection findings only appear in Cloud Logging, not SCC
C) Web Security Scanner requires a paid App Engine plan to generate findings
D) The organization is using the Standard tier of Security Command Center, which does not include those finding sources

<details>
<summary>Answer</summary>

**Correct: D**

SCC Standard includes only Security Health Analytics (and limited free detectors) at the organization level; Event Threat Detection, Container Threat Detection, VM Threat Detection, and full Web Security Scanner are Premium/Enterprise finding sources.

**Why the others are wrong:**
- Event Threat Detection appearing for one project suggests it's enabled somewhere (e.g., a Premium-scoped project), not a global misconfiguration.
- Container Threat Detection is designed to surface findings directly in SCC, not only in Cloud Logging.
- Web Security Scanner's managed scans work regardless of App Engine billing tier; availability is gated by SCC tier, not App Engine plan.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** Which Cloud Audit Log type records that IAM denied a request due to an organization policy or VPC Service Controls perimeter violation?

A) Admin Activity
B) Policy Denied
C) System Event
D) Data Access

<details>
<summary>Answer</summary>

**Correct: B**

Policy Denied audit logs record when a Google Cloud service denies access to a user or service account because of a security policy violation, such as an org policy constraint or VPC-SC perimeter rule.

**Why the others are wrong:**
- Admin Activity logs record successful configuration/metadata-changing API calls, not denials.
- System Event logs record Google-initiated automated actions (e.g., live migration), unrelated to policy enforcement.
- Data Access logs record read/write operations on user data, not policy-based denials.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** A team enabled a custom log bucket and wants analysts to run SQL joins across VPC Flow Logs and Cloud Audit Logs stored in that bucket without exporting to BigQuery first. Which feature should they use?

A) Log Router with a Pub/Sub sink
B) Cloud Monitoring MQL queries
C) Log Analytics on the log bucket
D) Data Access log export to Cloud Storage

<details>
<summary>Answer</summary>

**Correct: C**

Log Analytics lets you upgrade a log bucket to run BigQuery-style SQL queries directly against the logs stored in it (including joins across log types) without a separate BigQuery export step.

**Why the others are wrong:**
- Log Router/Pub/Sub sinks move logs to another destination but don't provide an in-place SQL query interface.
- Cloud Monitoring MQL is for time-series metrics, not for SQL querying of structured log entries.
- Exporting Data Access logs to Cloud Storage produces flat files, requiring separate tooling (e.g., BigQuery load jobs) to run SQL, adding an extra step the scenario wants to avoid.

</details>

---

**Q7.** Which statement about Cloud Audit Log retention and cost is correct?

A) All audit log types are stored in the _Default bucket for 30 days and billed per GB ingested
B) Data Access logs are always disabled and cannot be enabled for any service
C) Admin Activity, System Event, and Policy Denied logs are stored in the _Required bucket for 400 days at no charge and cannot be disabled
D) System Event logs can be excluded via an exclusion filter to reduce cost, similar to Data Access logs

<details>
<summary>Answer</summary>

**Correct: C**

Admin Activity, System Event, and Policy Denied logs write to the immutable _Required bucket, are retained 400 days, incur no storage charge, and cannot be turned off.

**Why the others are wrong:**
- Data Access logs (not all types) go to the _Default bucket with a 30-day default retention and are billed on ingestion once enabled — Admin Activity/System Event/Policy Denied are in _Required, not _Default.
- BigQuery Data Access logs are enabled by default, and Data Access logs for most other services CAN be enabled — they aren't universally disabled.
- System Event logs cannot be excluded or disabled; only Data Access (and, to a limited extent via filters, some Policy Denied volume) logs can be filtered to control cost.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** A company running workloads across a shared VPC wants visibility into which firewall rules are allowing or denying specific connections at the packet level, for troubleshooting a blocked application. Which capability should they enable?

A) VPC Flow Logs sampling at 50%
B) Cloud IDS with a CRITICAL severity threshold
C) Firewall Rules Logging (Cloud NGFW logs) on the relevant rules
D) Access Transparency logs

<details>
<summary>Answer</summary>

**Correct: C**

Firewall Rules Logging (part of Cloud NGFW) records each connection matched against a firewall rule, showing exactly whether it was allowed or denied — precisely what's needed to troubleshoot blocked traffic caused by rule matching.

**Why the others are wrong:**
- VPC Flow Logs show flow metadata but do not indicate which specific firewall rule allowed or denied the connection.
- Cloud IDS detects malicious payloads/signatures, not firewall rule match/deny decisions.
- Access Transparency logs Google support personnel's access to customer content, unrelated to firewall rule evaluation.

</details>

---

