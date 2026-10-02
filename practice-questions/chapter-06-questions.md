## Chapter 6: Sensitive Data Protection, Secret Manager, and Data Service Access Controls — Practice Questions

**Q1.** A company wants to share BigQuery query results with an external partner. The partner must see only three aggregated columns and must never be able to query the raw underlying table directly. Which approach BEST meets this requirement?

A) Grant the partner roles/bigquery.dataViewer directly on the source dataset
B) Export the three columns nightly to a Cloud Storage bucket and grant the partner a signed URL
C) Create an authorized view in a separate dataset that selects only the three columns, and grant the partner BigQuery Data Viewer on that dataset only
D) Add an IAM condition to the source table restricting access to business hours

<details>
<summary>Answer</summary>

**Correct: C**

An authorized view lets a view in a separate dataset query the source table using the view's own permissions, so the partner only needs access to the view's dataset and never touches the source table directly, satisfying both column restriction and no-raw-access requirements.

**Why the others are wrong:**
- Granting dataViewer on the source dataset exposes all columns and all rows, not just the three approved columns
- Exporting to GCS creates a stale copy management burden and loses live query capability; it doesn't restrict columns dynamically
- IAM conditions can restrict by time or resource tags but do not restrict which columns are visible

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A support team needs to view credit card numbers with only the last four digits visible; the rest must be irreversibly obscured for display purposes only. Which Sensitive Data Protection transformation should be used?

A) CharacterMaskConfig masking all but the last four characters
B) CryptoReplaceFfxFpeConfig with a wrapped Cloud KMS key
C) CryptoHashConfig with SHA-256
D) DateShiftConfig applied to the transaction date field

<details>
<summary>Answer</summary>

**Correct: A**

CharacterMaskConfig replaces characters with a masking character (e.g., X) while allowing you to specify how many characters from the end to leave unmasked, exactly matching the last-four-digits display requirement with no reversibility needed.

**Why the others are wrong:**
- CryptoReplaceFfxFpeConfig produces a reversible format-preserving token, which is unnecessary complexity when simple masking is required
- CryptoHashConfig produces a fixed-length hash output, destroying the format and making 'last four digits' display impossible
- DateShiftConfig only shifts date values and has no application to credit card numbers

</details>

---

**Q3.** An organization runs Cloud SQL for MySQL and wants database users authenticated using their existing Google identities instead of managing separate database passwords. Which feature should they enable, and what limitation should they plan for?

A) Cloud SQL Auth Proxy with SSL certificates; no username limitation applies
B) IAM database authentication; MySQL usernames are capped at 32 characters, which can truncate long IAM email addresses
C) Private Service Connect; usernames are unrestricted
D) VPC Service Controls; usernames must match the service account's unique numeric ID

<details>
<summary>Answer</summary>

**Correct: B**

IAM database authentication for Cloud SQL for MySQL (and PostgreSQL) lets users connect using IAM credentials, but MySQL's native 32-character username limit means long IAM principal emails get truncated, a well-known exam gotcha requiring careful naming or use of PostgreSQL instead.

**Why the others are wrong:**
- Cloud SQL Auth Proxy secures the connection transport but does not by itself provide IAM-based database user authentication
- Private Service Connect addresses private network connectivity, not database authentication
- VPC Service Controls create a security perimeter around APIs, unrelated to database username mapping

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** A data science team needs to anonymize customer SSNs in a training dataset. The transformation must be completely irreversible, even by the security team, while still producing a consistent fixed-length output for the same input. Which de-identification technique should be selected?

A) CryptoDeterministicConfig
B) CryptoReplaceFfxFpeConfig
C) CryptoHashConfig
D) ReplaceValueConfig with a static placeholder

<details>
<summary>Answer</summary>

**Correct: C**

CryptoHashConfig applies a one-way cryptographic hash, producing consistent output for identical input but with no key-based mechanism to reverse it, satisfying the 'irreversible even by security team' requirement.

**Why the others are wrong:**
- CryptoDeterministicConfig is reversible by anyone holding the wrapped crypto key and surrogate info type, violating the irreversibility requirement
- CryptoReplaceFfxFpeConfig format-preserving encryption is explicitly reversible with the encryption key
- ReplaceValueConfig with a static placeholder loses consistency across distinct input values (all values collapse to the same placeholder), breaking analytical usefulness

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** A healthcare company must pseudonymize patient IDs so that the same patient always maps to the same token across multiple BigQuery tables, and authorized investigators must be able to reverse the token back to the original ID when required. Which configuration should be used?

A) CryptoHashConfig with a salted SHA-256 key
B) BucketingConfig with fixed-size buckets
C) RedactConfig applied per column
D) CryptoDeterministicConfig referencing a wrapped Cloud KMS key and surrogate info type

<details>
<summary>Answer</summary>

**Correct: D**

CryptoDeterministicConfig produces the same token for the same input (consistent across tables) and is reversible via re-identification using the same wrapped key and surrogate info type, exactly matching the reversible-pseudonymization requirement.

**Why the others are wrong:**
- CryptoHashConfig is one-way and cannot be reversed for investigators
- BucketingConfig generalizes values into ranges and discards the specific identifier, making per-patient reversal impossible
- RedactConfig removes the value entirely, eliminating any possibility of re-identification

</details>

---

**Q6.** A regulated company must ensure a secret in Secret Manager is stored ONLY in us-central1 and us-east1 for data residency compliance. Which replication configuration should be used?

A) User-managed replication specifying exactly those two locations
B) Automatic replication
C) Automatic replication combined with a VPC Service Controls perimeter
D) Automatic replication combined with an organization policy restricting resource locations

<details>
<summary>Answer</summary>

**Correct: A**

User-managed replication is the only mode that lets you explicitly pin secret storage to a specific list of Google Cloud locations, which is required for strict data residency guarantees.

**Why the others are wrong:**
- Automatic replication lets Google choose storage locations for availability and does not guarantee residency to specific regions
- VPC Service Controls restricts API access paths, not the physical storage location of secret data
- An org policy restricting resource locations affects resource creation more broadly but does not replace explicit user-managed replication configuration for Secret Manager

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** A Cloud Storage bucket must serve some objects publicly (e.g., marketing images) while keeping other objects in the same bucket private, with per-object access differing based on the individual object owner. Which access control configuration should be used?

A) Enable uniform bucket-level access and use only IAM bindings
B) Use a signed URL for every object regardless of visibility
C) Disable uniform bucket-level access (fine-grained access) and use per-object ACLs alongside bucket-level IAM
D) Apply an IAM Deny policy on the bucket for all unauthenticated users

<details>
<summary>Answer</summary>

**Correct: C**

Fine-grained access control keeps ACLs enabled, allowing individual objects to have distinct per-object permissions layered on top of bucket-level IAM — the only way to mix public and private access at the object level within one bucket.

**Why the others are wrong:**
- Uniform bucket-level access disables ACLs entirely, so all objects would inherit the same bucket-level IAM policy, preventing per-object public/private mixing
- Signed URLs work for temporary controlled access but are impractical for permanently public objects and add unnecessary operational overhead
- An IAM Deny policy blocking all unauthenticated users would break the requirement to serve some objects publicly

</details>

---

**Q8.** Which statement correctly differentiates BigQuery authorized views from Cloud Storage ACLs?

A) Authorized views grant row and column-level filtering natively without any additional configuration, while ACLs only ever apply at the bucket level
B) Authorized views and ACLs are functionally identical, differing only in syntax
C) Authorized views allow a view to query a source table on behalf of a grantee without giving direct table access, while ACLs grant direct object or bucket-level permissions to specific identities
D) ACLs are the BigQuery mechanism, while authorized views apply only to Cloud Storage objects

<details>
<summary>Answer</summary>

**Correct: C**

An authorized view acts as an intermediary that is granted access to the source table so the end user never needs direct permissions on that table, whereas Cloud Storage ACLs grant permissions directly on the object or bucket resource itself to specific principals.

**Why the others are wrong:**
- Authorized views alone do not provide row/column filtering — that requires row access policies or policy tags configured separately
- The mechanisms are structurally different (proxy access vs. direct resource-level grants), not just syntactic variants
- This reverses the services — ACLs are a Cloud Storage/legacy BigQuery concept, and authorized views are specific to BigQuery

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

