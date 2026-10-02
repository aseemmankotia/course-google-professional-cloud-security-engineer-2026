## Chapter 8: Cloud Storage Lifecycle and Securing AI/ML Workloads — Practice Questions

**Q1.** A financial services company must retain transaction audit logs in Cloud Storage for exactly 7 years. Regulators require that NO ONE, including project owners, be able to shorten or delete this retention early. Which configuration meets this requirement?

A) Enable Object Versioning and create a lifecycle rule that deletes noncurrent versions after 7 years
B) Apply a temporary hold to every object and instruct staff to never release it
C) Create a Deny IAM policy on storage.objects.delete for all principals
D) Set a 7-year bucket retention policy and then lock the bucket (Bucket Lock)

<details>
<summary>Answer</summary>

**Correct: D**

A locked retention policy is enforced by the storage system itself, cannot be reduced or removed by anyone (not even the org admin), and the bucket cannot even be deleted until all objects satisfy the retention period — true WORM compliance.

**Why the others are wrong:**
- Versioning + lifecycle delete still allows an admin to edit or remove the lifecycle rule at any time, so it is not immutable enforcement.
- Temporary holds are manually managed per object and can be released by anyone with storage.objects.update permission — not a system-enforced guarantee.
- A Deny IAM policy can be edited or removed by any principal with sufficient IAM Admin permissions, so it does not provide unbreakable retention.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** Your organization trains proprietary models on Vertex AI using data stored in Cloud Storage and BigQuery. Security requires that this training data can never be exfiltrated to an external project or the public internet, even by a compromised service account with valid IAM permissions. Which control should you implement?

A) Firewall rules restricting egress from the training VPC
B) A VPC Service Controls service perimeter containing the Cloud Storage, BigQuery, and Vertex AI APIs
C) Private Google Access enabled on the training subnet
D) Cloud Armor security policy attached to the Vertex AI endpoint

<details>
<summary>Answer</summary>

**Correct: B**

VPC Service Controls creates a context-aware perimeter around API-level access to services like Cloud Storage, BigQuery, and Vertex AI, blocking data movement to resources outside the perimeter regardless of valid IAM credentials — this is the only listed option that stops exfiltration at the API/data-plane layer.

**Why the others are wrong:**
- Firewall rules only control network-layer traffic and do not restrict API calls made over Google's public API endpoints.
- Private Google Access only lets VMs without external IPs reach Google APIs privately — it does not prevent data exfiltration to another project's resources.
- Cloud Armor protects HTTP(S) load balancers from web attacks (e.g., DDoS, WAF rules) and has no role in preventing API-based data exfiltration.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** You need a Cloud Storage lifecycle rule that transitions objects to Coldline storage after 90 days and permanently deletes them after 365 days. Which rule set is correct?

A) {"action":{"type":"SetStorageClass","storageClass":"COLDLINE"},"condition":{"createdBefore":"90"}} and {"action":{"type":"Archive"},"condition":{"age":365}}
B) {"action":{"type":"Coldline"},"condition":{"daysSinceCustomTime":90}} and {"action":{"type":"Delete"},"condition":{"numNewerVersions":365}}
C) {"action":{"type":"SetStorageClass","storageClass":"COLDLINE"},"condition":{"age":90}} and {"action":{"type":"Delete"},"condition":{"age":365}}
D) {"action":{"type":"SetStorageClass"},"condition":{"isLive":true,"age":90}} and {"action":{"type":"Delete"},"condition":{"isLive":false}}

<details>
<summary>Answer</summary>

**Correct: C**

Lifecycle rules use the age condition (days since object creation) with a SetStorageClass action specifying storageClass:COLDLINE, and a separate Delete action rule with age:365. This is valid, minimal, and matches the stated requirement exactly.

**Why the others are wrong:**
- createdBefore expects an RFC 3339 date string, not a bare number like "90", and Archive is not a valid action type (only SetStorageClass or Delete).
- Coldline is not a valid action type, and numNewerVersions counts object versions, not elapsed days — this rule would behave unpredictably.
- isLive alone does not express '90 days' or '365 days' timing and omits the required age condition, so the transition/delete timing described would not be enforced.

</details>

---

**Q4.** Your company trains a Vertex AI model using EU-resident customer data. Compliance requires that this training data and any processing never leave the EU, and this restriction must be enforced organization-wide, not just documented in a runbook. What should you configure?

A) An Organization Policy constraint (gcp.resourceLocations) restricting resource creation to EU regions, combined with EU-based regional Cloud Storage buckets and Vertex AI regional endpoints
B) Rely on your team choosing EU regions when creating each Vertex AI resource
C) A VPC Service Controls perimeter with an access level restricted to EU IP ranges
D) A Cloud DLP inspection job that flags any EU data found outside the EU

<details>
<summary>Answer</summary>

**Correct: A**

The gcp.resourceLocations organization policy constraint enforces at the platform level where new resources (buckets, Vertex AI datasets/endpoints) can be created, and pairing it with EU-region resources and regional Vertex AI endpoints ensures data residency is systemically enforced, not dependent on manual choices.

**Why the others are wrong:**
- Relying on staff to manually pick regions is not an enforced control and fails any audit requiring technical enforcement.
- VPC-SC access levels restrict WHO/WHERE can call the API from a network perspective — they do not restrict WHICH region a resource is created or processed in.
- DLP inspection is detective, not preventive — it flags after the fact rather than stopping data from ever leaving the EU.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** Before uploading a customer support transcript dataset to a Cloud Storage bucket for Vertex AI fine-tuning, you must ensure any embedded SSNs and credit card numbers are removed or masked. Which Google Cloud service should you use?

A) Cloud Armor
B) Sensitive Data Protection (Cloud DLP)
C) Binary Authorization
D) Security Command Center

<details>
<summary>Answer</summary>

**Correct: B**

Sensitive Data Protection (Cloud DLP) provides infoType detectors for PII like SSNs and credit card numbers, plus de-identification transformations (masking, tokenization, redaction) that can run as a job on the dataset before it's used for training.

**Why the others are wrong:**
- Cloud Armor is a WAF/DDoS protection service for HTTP(S) load balancers and has no data inspection capability.
- Binary Authorization enforces that only trusted, signed container images are deployed — it has nothing to do with dataset content inspection.
- Security Command Center aggregates security findings and posture across the org; it does not perform content-level PII inspection or redaction of a dataset.

</details>

---

**Q6.** Your team deployed a Vertex AI model to an endpoint for real-time prediction. Security requires that the endpoint never be reachable from the public internet and only accessible from workloads inside your VPC. What should you configure?

A) An IAM Condition restricting aiplatform.endpoints.predict to internal service accounts
B) Deploy the model to a private endpoint using VPC Network Peering or Private Service Connect instead of the public endpoint
C) A VPC Service Controls ingress rule allowing only the on-prem CIDR block
D) A Cloud Armor security policy allowing only internal IP ranges

<details>
<summary>Answer</summary>

**Correct: B**

Vertex AI supports deploying prediction endpoints as private endpoints, reachable only over VPC Network Peering or Private Service Connect, eliminating any public IP exposure at the network layer — this is the only option that removes the endpoint from the public internet entirely.

**Why the others are wrong:**
- IAM Conditions restrict WHO can call the API but do nothing to change the network reachability of the public prediction endpoint.
- VPC-SC ingress rules control cross-perimeter API access context, not the network exposure of the prediction serving endpoint itself.
- Cloud Armor applies to HTTP(S) load balancers you front yourself; the default Vertex AI public prediction endpoint is a Google-managed API endpoint that Cloud Armor cannot be attached to.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** Your organization deploys a Gemini Enterprise agent that grounds its answers using an internal HR policy document store. Only members of the HR group should receive grounded answers from that data; other employees should not have their queries grounded on that data at all. How should you enforce this?

A) Rely on the model's built-in safety filters to withhold HR content from non-HR users
B) Wrap the entire Gemini Enterprise platform in a VPC Service Controls perimeter
C) Configure the agent's grounding data store connector and its runtime service account IAM roles so only the HR group has read access to that data store; scope the agent's permissions accordingly
D) Apply a Sensitive Data Protection de-identification transform to every agent response

<details>
<summary>Answer</summary>

**Correct: C**

Gemini Enterprise agent access control is enforced by scoping IAM on the underlying grounding data source (e.g., data store, Drive folder, BigQuery dataset) and the identity/service account the agent runs as — access to grounded data is governed the same way as any other Google Cloud resource access.

**Why the others are wrong:**
- Safety filters address harmful or policy-violating content generation, not authorization to specific business data — they are not an access control mechanism.
- A VPC-SC perimeter protects against data exfiltration across projects/networks; it does not implement per-group authorization to a specific grounding data source.
- De-identification would strip out the very HR information the agent is meant to legitimately provide to authorized HR users — it doesn't solve group-based access control.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** Which statement correctly distinguishes an event-based hold from a temporary hold on a Cloud Storage object?

A) An event-based hold restarts the bucket's retention-period countdown for that object once the hold is released; a temporary hold simply blocks deletion/overwrite until manually released and does not affect the retention countdown
B) A temporary hold restarts the retention countdown once released; an event-based hold blocks deletion indefinitely with no countdown involved
C) Both hold types behave identically and only differ in the IAM permission required to set them
D) A temporary hold can only be applied to locked buckets, while an event-based hold requires an unlocked retention policy

<details>
<summary>Answer</summary>

**Correct: A**

Event-based hold is designed for cases where the retention clock should only start once a triggering event occurs (e.g., a document is finalized); releasing it starts the bucket's retention period. Temporary hold is a simple on/off block on deletion/overwrite (commonly used for litigation holds) with no interaction with the retention-period countdown.

**Why the others are wrong:**
- This reverses the actual behavior — it is the event-based hold, not the temporary hold, that triggers the retention countdown upon release.
- They differ functionally, not just by required permission — event-based hold interacts with retention timing while temporary hold does not.
- Both hold types can be applied regardless of whether the bucket's retention policy is locked; hold status is independent of Bucket Lock state.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

