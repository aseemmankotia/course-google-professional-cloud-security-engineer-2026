## Chapter 7: Encryption Everywhere: CMEK, Cloud EKM, and Confidential Computing — Practice Questions

**Q1.** A defense contractor requires that encryption keys protecting their Cloud Storage data NEVER reside on Google-managed infrastructure, even in encrypted form, and must be revocable instantly by disconnecting network access to the key manager. Which solution BEST meets this requirement?

A) Customer-managed encryption keys (CMEK) with Cloud HSM protection level
B) Default Google-managed encryption with a Cloud Storage access policy
C) CMEK with imported key material (BYOK) using an import job
D) Cloud External Key Manager (Cloud EKM) with keys hosted at an external partner

<details>
<summary>Answer</summary>

**Correct: D**

Cloud EKM is the only option where key material is stored and managed entirely outside Google Cloud infrastructure at a supported external partner (e.g., Thales, Fortanix). Google calls out to the external manager for every crypto operation, so cutting network access to the EKM partner immediately revokes Google's ability to use the key.

**Why the others are wrong:**
- Cloud HSM still stores key material inside Google-managed HSMs, violating the 'never resides on Google infrastructure' requirement.
- Default encryption gives no customer control over key location or revocation at all.
- BYOK import still copies the wrapped key material into Cloud KMS, so it ends up stored on Google infrastructure.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A team is processing highly sensitive genomic data on Compute Engine and a security auditor flags that the data must be protected even while it is being actively processed in RAM, not just at rest or in transit. Which Google Cloud feature should they enable?

A) Customer-managed encryption keys (CMEK) on the boot disk
B) VPC Service Controls around the project
C) Confidential VM with memory encryption
D) Cloud KMS envelope encryption for application data

<details>
<summary>Answer</summary>

**Correct: C**

Confidential VM uses hardware-based memory encryption (e.g., AMD SEV/SEV-SNP) so that data being processed in memory is encrypted and isolated from the hypervisor. This directly addresses 'data in use' protection.

**Why the others are wrong:**
- CMEK on the boot disk only protects data at rest on disk, not data actively in memory.
- VPC Service Controls prevents data exfiltration across service boundaries, not memory-level encryption.
- Envelope encryption protects application-level data at rest/in transit but does not encrypt RAM contents.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** An architect wants a key rotation policy where Cloud KMS automatically generates a new primary key version as frequently as the platform allows, without any manual intervention. What is the minimum rotation period they can configure for a Cloud KMS key?

A) 1 hour
B) 24 hours
C) 7 days
D) 90 days

<details>
<summary>Answer</summary>

**Correct: B**

Cloud KMS enforces a minimum automatic rotation period of 24 hours (one day); attempting to set anything shorter is rejected by the API.

**Why the others are wrong:**
- 1 hour is below the platform's enforced minimum and is not a valid rotation period.
- 7 days is a valid but not the minimum allowed rotation period.
- 90 days is Google's commonly suggested default rotation period, not the minimum.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** A security engineer disables a Cloud KMS key version after discovering suspicious API calls, but later needs to permanently guarantee the key material can never be used again after an investigation completes. What is the correct sequence of actions?

A) Destroy the key version immediately since disable and destroy have identical effects
B) Delete the entire key ring to remove all traces of the key
C) Disable the key version (reversible) then later destroy it (irreversible after the scheduled window)
D) Rotate the key, which automatically destroys the old key version

<details>
<summary>Answer</summary>

**Correct: C**

Disabling a key version is reversible and stops it from being used for crypto operations immediately, buying time for investigation. Destroying schedules irreversible deletion of key material after a configurable window (default 24 hours), permanently making protected data unrecoverable.

**Why the others are wrong:**
- Disable is reversible (can re-enable); destroy is not — they are not identical, and destroying immediately removes the ability to undo the action if the investigation reveals it was a false alarm.
- Key rings cannot be deleted in Cloud KMS; only individual key versions can be destroyed.
- Rotation creates a new primary version but does not automatically destroy old versions — they remain until explicitly disabled or destroyed.

</details>

---

**Q5.** A startup wants encryption at rest for all of its BigQuery datasets with zero configuration effort and no need to manage key lifecycle, rotation, or access policies for keys. What should they rely on?

A) Google default encryption (Google-managed keys)
B) Cloud External Key Manager (EKM)
C) Customer-managed encryption keys (CMEK) with manual rotation
D) Confidential BigQuery with imported keys

<details>
<summary>Answer</summary>

**Correct: A**

Google applies default encryption at rest to all data automatically using Google-managed keys — there is no setup, IAM key policy, or rotation schedule for the customer to manage.

**Why the others are wrong:**
- Cloud EKM requires standing up and managing an external key manager, the opposite of zero effort.
- CMEK requires creating and managing Cloud KMS key rings, keys, IAM bindings, and rotation policy.
- 'Confidential BigQuery with imported keys' is not a real product combination and still implies key management overhead.

</details>

---

**Q6.** A bank has existing AES-256 key material generated and controlled by their on-premises HSM and wants to bring that exact key material into Cloud KMS to protect Cloud SQL data, rather than letting Google generate a new key. Which Cloud KMS feature enables this?

A) Cloud HSM protection level with auto-generated keys
B) Cloud EKM with an external partner integration
C) Default encryption with a customer-supplied encryption context
D) An import job using a wrapped key with RSA-OAEP and AES-256 key wrapping

<details>
<summary>Answer</summary>

**Correct: D**

Bring Your Own Key (BYOK) in Cloud KMS is implemented via an import job: the customer wraps their existing key material using an approved RSA-OAEP wrapping algorithm with AES-256 key wrap, then imports it into a key version, preserving the original key bytes.

**Why the others are wrong:**
- Cloud HSM with auto-generated keys creates brand-new key material inside Google's HSMs — it does not preserve the customer's existing key.
- Cloud EKM keeps the key entirely outside Google Cloud rather than importing it in — this is a different pattern from BYOK import.
- 'Customer-supplied encryption context' is not a mechanism for bringing existing key material into Cloud KMS.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** Which Cloud KMS protection level indicates that key operations are performed by an external key manager reachable only over a private VPC connection, rather than the public internet?

A) HSM
B) EXTERNAL_VPC
C) SOFTWARE
D) EXTERNAL

<details>
<summary>Answer</summary>

**Correct: B**

EXTERNAL_VPC is the Cloud EKM protection level where calls to the external key manager traverse a private VPC connection (via a service attachment), avoiding the public internet path used by the plain EXTERNAL protection level.

**Why the others are wrong:**
- HSM indicates keys are stored in Google-managed FIPS 140-2 Level 3 HSMs, not an external manager.
- SOFTWARE indicates standard software-protected keys stored within Cloud KMS, not external.
- EXTERNAL is the Cloud EKM protection level that reaches the external key manager over the public internet rather than a private VPC path.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** Which statement correctly differentiates Confidential VM from a standard Compute Engine VM using CMEK-encrypted persistent disks?

A) Confidential VM and CMEK-encrypted disks provide identical protection because both use AES-256
B) CMEK-encrypted disks also encrypt VM memory contents automatically
C) Confidential VM replaces the need for disk encryption entirely
D) Confidential VM encrypts data in use in memory using hardware-based encryption, while CMEK on standard VMs only encrypts data at rest on disk

<details>
<summary>Answer</summary>

**Correct: D**

Confidential VM adds memory encryption via the CPU (e.g., AMD SEV), protecting data actively being processed, which is a distinct threat model from at-rest disk encryption provided by CMEK on persistent disks.

**Why the others are wrong:**
- Both may use AES-256 internally, but they protect different data states (in use vs at rest) and are not equivalent protections.
- CMEK on disks has no effect on RAM contents; memory is not encrypted by disk-level CMEK.
- Confidential VM still uses disk encryption (default or CMEK) for persistent disks — it adds memory protection, it doesn't replace disk encryption.

</details>

---

