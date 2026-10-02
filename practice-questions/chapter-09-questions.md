## Chapter 9: Binary Authorization, CVE Scanning, and Security Posture Management — Practice Questions

**Q1.** A platform team requires that GKE only deploys images signed by both the QA attestor and the Security attestor after Artifact Analysis reports no CRITICAL vulnerabilities. They also need a documented emergency bypass for production incidents that still gets logged. Which approach meets both requirements?

A) Set Binary Authorization enforcementMode to DRYRUN_AUDIT_LOG_ONLY permanently so deployments are never blocked
B) Disable Binary Authorization on the cluster and rely on RBAC to restrict who can deploy
C) Grant the on-call engineer the Kubernetes Engine Admin role so they can bypass admission control
D) Configure a Binary Authorization policy requiring attestations from both attestors, and use the documented break-glass annotation only during incidents

<details>
<summary>Answer</summary>

**Correct: D**

REQUIRE_ATTESTATION with ENFORCED_BLOCK_AND_AUDIT_LOG enforces both attestors, and the documented break-glass annotation bypasses the check for genuine emergencies while still recording the event in audit logs.

**Why the others are wrong:**
- DRYRUN mode never blocks anything, so it doesn't enforce the dual-attestation requirement at all
- Disabling Binary Authorization removes the control entirely instead of providing a controlled, logged exception
- Elevating IAM role to Kubernetes Engine Admin bypasses admission control silently and isn't a documented, logged break-glass mechanism

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A team wants Cloud Run to reject any revision whose container image has not passed vulnerability scanning with zero CRITICAL findings and is not cryptographically signed. Which configuration achieves this at deploy time?

A) Add a manual approval step in the Cloud Build trigger only
B) Enable VPC Service Controls on the Artifact Registry repository
C) Add an IAM Condition restricting the Cloud Run Deployer role by time of day
D) Enable Binary Authorization on the Cloud Run service and require an attestation from an attestor whose signing step only runs after Artifact Analysis confirms no CRITICAL vulnerabilities

<details>
<summary>Answer</summary>

**Correct: D**

Binary Authorization on Cloud Run enforces that a valid attestation exists before deploy; gating the signing step on a clean Artifact Analysis scan ties vulnerability status directly to deployability.

**Why the others are wrong:**
- A manual approval step doesn't automatically verify scan results or cryptographic signatures
- VPC Service Controls restricts data exfiltration across service perimeters, not image vulnerability gating
- Time-based IAM conditions restrict who/when can deploy, not whether the image itself is secure

</details>

---

**Q3.** An organization needs a guardrail, beyond any predefined Organization Policy constraint, that blocks creation or update of any GKE cluster with legacy ABAC authorization enabled. How should the security team implement this?

A) Define a custom Organization Policy constraint with a CEL condition referencing the cluster's legacy ABAC field, methodTypes CREATE and UPDATE, and actionType DENY
B) Write an IAM deny policy targeting container.clusters.create
C) Create a VPC Service Controls perimeter around the GKE API
D) Enable the Security Health Analytics module for legacy authorization and rely on its findings to manually delete clusters

<details>
<summary>Answer</summary>

**Correct: A**

Custom Organization Policy constraints use CEL expressions against resource fields and can enforce DENY on CREATE/UPDATE calls, providing preventive (not just detective) enforcement for conditions not covered by predefined constraints.

**Why the others are wrong:**
- IAM deny policies control who can call an API, not the configuration of the resource being created
- VPC Service Controls governs data perimeter/API surface exposure, not resource configuration attributes
- Security Health Analytics is detective (finds misconfigurations after the fact), not preventive enforcement

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** Which GCP capability continuously rescans container images already stored in Artifact Registry as new CVEs are disclosed, without requiring a new push?

A) Artifact Analysis (Container Analysis) continuous vulnerability scanning
B) Web Security Scanner
C) Security Health Analytics
D) VPC Service Controls

<details>
<summary>Answer</summary>

**Correct: A**

Artifact Analysis performs on-push scanning and continues to monitor stored images against newly published vulnerability data for as long as the image remains in the registry.

**Why the others are wrong:**
- Web Security Scanner scans deployed web applications for OWASP-style issues, not container images
- Security Health Analytics reports cloud misconfigurations, not container CVEs
- VPC Service Controls is a network/data perimeter control unrelated to vulnerability scanning

</details>

---

**Q5.** Which service within Security Command Center provides managed detectors for misconfigurations such as public storage buckets, open firewalls, and missing MFA, mapped to compliance benchmarks like CIS?

A) Artifact Analysis
B) Security Health Analytics
C) Binary Authorization
D) Cloud Asset Inventory

<details>
<summary>Answer</summary>

**Correct: B**

Security Health Analytics is the built-in Security Command Center service that continuously evaluates resource configurations against a set of managed detectors and compliance benchmarks.

**Why the others are wrong:**
- Artifact Analysis scans container images for vulnerabilities, not general resource misconfigurations
- Binary Authorization enforces deploy-time image provenance, it does not scan for misconfigurations
- Cloud Asset Inventory catalogs resource metadata and history but does not generate security posture findings itself

</details>

---

**Q6.** Which Binary Authorization evaluationMode and enforcementMode combination lets a team test a new attestation requirement without blocking any deployments, while still recording violations?

A) REQUIRE_ATTESTATION with ENFORCED_BLOCK_AND_AUDIT_LOG
B) REQUIRE_ATTESTATION with DRYRUN_AUDIT_LOG_ONLY
C) ALWAYS_DENY with DRYRUN_AUDIT_LOG_ONLY
D) ALWAYS_ALLOW with ENFORCED_BLOCK_AND_AUDIT_LOG

<details>
<summary>Answer</summary>

**Correct: B**

REQUIRE_ATTESTATION defines the check to be evaluated, and DRYRUN_AUDIT_LOG_ONLY logs violations without blocking, which is the standard safe rollout pattern for new policies.

**Why the others are wrong:**
- ENFORCED_BLOCK_AND_AUDIT_LOG would actively block non-compliant deployments, defeating the goal of a safe test rollout
- ALWAYS_DENY blocks everything regardless of attestation, which is not a testable evaluation of the new requirement
- ALWAYS_ALLOW never evaluates attestation requirements at all, so nothing meaningful is tested

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** When Binary Authorization creates an attestation for a container image, what does it cryptographically sign to guarantee the reference cannot later be repointed to a different image?

A) The image tag, such as :latest
B) The Cloud Build build ID that produced the image
C) The Container Analysis note ID only, without image data
D) The image digest (SHA-256 hash of the image content)

<details>
<summary>Answer</summary>

**Correct: D**

Attestations bind to the immutable image digest, ensuring the signed reference cannot be silently swapped the way a mutable tag could be.

**Why the others are wrong:**
- Tags are mutable and can be repointed to a different image after signing, which would invalidate the security guarantee
- The build ID identifies the pipeline run, not the specific image content being deployed
- The note ID references the Container Analysis metadata container but is not itself the signed artifact reference

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** What is the primary security benefit of using a distroless base image instead of a full OS base image like ubuntu:latest for a production container?

A) It produces faster build times with no other benefit
B) It automatically patches vulnerabilities in application dependencies
C) It reduces attack surface by removing the shell, package manager, and unnecessary OS binaries an attacker could exploit
D) It is required for Binary Authorization attestations to be generated

<details>
<summary>Answer</summary>

**Correct: C**

Distroless images strip out shells, package managers, and non-essential OS utilities, minimizing the tools available to an attacker after compromising a container and reducing the CVE surface from unused packages.

**Why the others are wrong:**
- Faster builds may be a side effect but are not the primary security rationale tested on the exam
- Distroless images don't auto-patch dependencies — application libraries still require separate patch management
- Binary Authorization attestation generation works with any base image; it is unrelated to the image's OS composition

</details>

---

