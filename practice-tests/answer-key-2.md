# Answer Key — Practice Test 2

**Q1. Correct: C**

IAM deny policies are evaluated before allow policies and take precedence regardless of any allow grant, including inherited Editor roles. Scoping the deny policy to the folder and targeting only the contractors principal set blocks the delete permission for that group without touching the shared org-level Editor binding.

---

**Q2. Correct: D**

Workforce Identity Federation pool providers support attribute mappings (to map SAML/OIDC assertion attributes to Google attributes like google.subject and google.groups) and attribute conditions, which are CEL expressions evaluated at federation time. Setting a condition on the assertion attribute rejects federation entirely for any subject that doesn't match, before any Google Cloud IAM evaluation occurs.

---

**Q3. Correct: D**

Privileged Access Manager lets administrators define entitlements (which role, which resource scope, eligible requesters) that require approval and automatically expire after a configured duration, implementing just-in-time privileged access with an audit trail — exactly matching the requirement for temporary, approved elevation.

---

**Q4. Correct: D**

Workload Identity Federation lets external workloads (like GitHub Actions, which issues OIDC tokens) exchange their token for short-lived Google Cloud credentials without any service account key. Attribute conditions on the provider can inspect claims like repository and ref (branch) to restrict which specific repo/branch is allowed to impersonate the target service account — satisfying both the no-keys requirement and the scoping requirement.

---

**Q5. Correct: B**

Access Context Manager access levels can combine multiple conditions — IP subnetworks and device policy attributes (such as requiring encryption and corporate ownership via endpoint verification/BeyondCorp) — into a single access level. This access level is then referenced in the VPC Service Controls perimeter's ingress policy, so access to the protected BigQuery API is denied unless both device and network context match, independent of IAM permissions.

---

**Q6. Correct: D**

Setting constraints/iam.disableServiceAccountKeyCreation at the organization enforces the restriction everywhere by default (inheritance), and organization policies support setting a more permissive override at a lower-level resource (the specific project) to carve out the temporary exception — exactly matching the requirement for org-wide enforcement with one scoped, time-limited exception.

---

**Q7. Correct: A**

IAM deny policies evaluate before allow policies and always win regardless of where the allow binding was granted. Deny policies support denyExceptionPrincipals, letting you carve out the break-glass group from an org-wide deny, which is exactly the 'deny everyone except X' pattern the exam tests.

---

**Q8. Correct: C**

Workload Identity Federation supports a dedicated AWS provider type that natively understands AWS SigV4-signed GetCallerIdentity assertions. Mapping google.subject to assertion.arn and adding an attribute condition on assertion.account/assertion.arn scopes federation to exactly one AWS account and role, avoiding any stored credentials.

---

**Q9. Correct: B**

Privileged Access Manager (PAM) is purpose-built for just-in-time privilege elevation: it defines entitlements for specific roles, supports required-approval workflows, and automatically revokes the grant after the configured activation window — with no standing binding existing until activated.

---

**Q10. Correct: B**

A basic condition within a single Access Level can combine multiple attributes (IP subnetworks, device policy, region, etc.) with implicit AND logic among the fields of that one condition, correctly enforcing that both the IP range and device-policy requirements are satisfied simultaneously before access is granted.

---

**Q11. Correct: A**

Cloud Identity lets admins scope 2-Step Verification enforcement to a specific OU (Super Admins) and restrict which second-factor methods are permitted. Selecting 'Security Key' only enforces a phishing-resistant hardware-backed factor and explicitly excludes SMS and backup codes, matching the requirement exactly.

---

**Q12. Correct: A**

Ingress rules govern requests originating from outside a perimeter that target protected resources inside it. Since the service account and Dataflow job live outside the perimeter and the BigQuery dataset is inside, an ingress rule scoped to that identity and that exact dataset grants the narrowest possible access.

---

**Q13. Correct: C**

Private Service Connect publishes a specific service via a service attachment; consumers connect through a PSC endpoint in their own VPC. There is no CIDR overlap constraint, the producer's full network is never exposed, and it scales to many independent consumer VPCs across organizations.

---

**Q14. Correct: B**

Dynamic Port Allocation lets each VM's port count scale between a configured minimum and maximum based on actual demand, so busy instances can burst above the static 64-port floor while idle instances release unused ports back to the pool — directly solving uneven exhaustion.

---

**Q15. Correct: A**

The 99.99% SLA topology requires redundancy on both sides: both interfaces of the HA VPN gateway must each connect to a peer device/interface, resulting in four tunnels total. This ensures no single point of failure on either the Google Cloud side or the on-premises side.

---

**Q16. Correct: B**

Secure Web Proxy performs Layer 7 URL/domain-based filtering for egress web traffic, supports TLS interception, and integrates with IAM to apply and log policy per calling identity such as a service account — matching both the domain-restriction and per-identity visibility requirements.

---

**Q17. Correct: D**

Response Policy Zones let administrators override the standard DNS response for specified domains — for example, returning NXDOMAIN for known-malicious domains or redirecting queries for internal names — providing a policy enforcement point at the DNS layer for boundary defense.

---

**Q18. Correct: D**

Private Google Access only applies to traffic originating from VM instances inside a VPC subnet with a default route to the internet gateway — it does not extend to on-premises hosts. To reach VPC-SC-protected APIs privately from on-premises over Interconnect, you publish a Private Service Connect endpoint bound to the restricted.googleapis.com VIP inside the VPC, then advertise that endpoint's IP via the Interconnect. Traffic arriving at that endpoint is treated as coming from inside the VPC/project, satisfying perimeter requirements without traversing the internet.

---

**Q19. Correct: A**

private.googleapis.com (199.36.153.8/30) provides Private Google Access to APIs but does NOT enforce or propagate VPC Service Controls context — only restricted.googleapis.com (199.36.153.4/30) is VPC-SC aware. If the DNS/route configuration still points at the private.googleapis.com VIP after a perimeter is enabled, calls to protected services are rejected because they arrive without the perimeter context, even with correct IAM.

---

**Q20. Correct: B**

Secure Web Proxy is the fully managed, Envoy-based forward proxy for filtering and logging outbound HTTP(S) traffic from VMs without external IPs. It supports URL Lists to allow only specific FQDNs/paths, TLS interception policies, and per-request logging — precisely matching the requirement for managed, URL-aware egress control.

---

**Q21. Correct: C**

Cloud NGFW Enterprise's threat intelligence feature integrates Google-curated, continuously updated feeds of known-malicious IPs and domains as dynamic address groups/FQDN objects, which can be referenced directly in firewall policy rules to automatically block C2 and malware traffic without manual list maintenance.

---

**Q22. Correct: B**

Cloud EKM is the only option where the key material never resides in Google Cloud at any point — Google Cloud makes an external call to the partner-hosted key manager for every cryptographic operation, and revoking access at the external system immediately blocks all future operations, satisfying the 'entirely outside Google' and 'instant revocation' requirements.

---

**Q23. Correct: C**

Format-preserving encryption (FPE), implemented in the DLP API using the FFX mode, encrypts a value while preserving its original format (length and character set), which is exactly what's needed to keep legacy validation logic intact while still protecting the underlying value.

---

**Q24. Correct: A**

Reading a secret's payload (accessSecretVersion) generates a Data Access audit log of type DATA_READ, and Data Access logs (other than BigQuery) are disabled by default for cost reasons. You must explicitly enable Data Access audit logging with the Data Read log type for the Secret Manager API in IAM & Admin > Audit Logs to capture these calls.

---

**Q25. Correct: B**

Confidential Space runs a workload inside a hardened, attested Confidential VM environment where neither the infrastructure operator, the data owners, nor the code author can access the data in the clear during processing. It uses workload identity federation and attestation to grant each party's data access only to a verified, trusted binary, making it purpose-built for multi-party confidential collaboration.

---

**Q26. Correct: C**

Key rotation in Cloud KMS only changes which key version is used for new encrypt operations going forward; it does not automatically re-encrypt existing ciphertext. Previously written objects remain protected by whichever key version was primary at write time, and that version must stay enabled for those objects to remain decryptable.

---

**Q27. Correct: D**

A locked Bucket Lock retention policy makes the retention period immutable and permanently enforced by Cloud Storage itself — it cannot be shortened or removed, even by the project owner or org admin, and objects cannot be deleted or overwritten before the retention period expires, which is exactly the compliance guarantee required.

---

**Q28. Correct: D**

Cloud EKM does not cache external key material in Google Cloud; every cryptographic operation is forwarded to the external key manager for that specific request. Disabling the key at the external system therefore takes effect on the very next crypto operation, without needing to delete any Google Cloud resource or destroy data.

---

**Q29. Correct: C**

CryptoReplaceFfxFpeConfig implements Format-Preserving Encryption (FFX mode), which is specifically designed to produce ciphertext output in the same length and character set (alphabet) as the input while remaining fully reversible by anyone holding access to the wrapping key — exactly matching both requirements in the scenario.

---

**Q30. Correct: B**

Confidential Space is a specialized Confidential VM-based trusted execution environment product designed specifically for multi-party data collaboration: it provides remote attestation so all parties (and the workload owner) can cryptographically verify that only the agreed, unmodified code ran on the data, and it isolates data and computation so that neither the counterparty nor Google/the cloud operator can access the data or model in use.

---

**Q31. Correct: A**

VPC Service Controls establishes a context-aware service perimeter around the specified APIs (BigQuery, Cloud Storage, Vertex AI/Gemini Enterprise); even if credentials are stolen, requests attempting to move data to a resource or project outside the perimeter are blocked at the network/API level, directly preventing data exfiltration regardless of what IAM permissions the compromised identity holds.

---

**Q32. Correct: C**

Producing a valid attestation requires a Container Analysis Note tied to the Attestor, plus signing the image digest with the KMS key referenced by that attestor's public key. Without this, Binary Authorization has nothing to verify.

---

**Q33. Correct: A**

An organization-level aggregated sink with includeChildren=true automatically captures logs from all current and future projects/folders, and an exclusion filter is the supported mechanism to omit a specific folder's logs from that export, minimizing ongoing management.

---

**Q34. Correct: D**

Cloud IDS is purpose-built for signature-based intrusion and malware detection. It relies on Packet Mirroring to analyze a copy of traffic out-of-band, so it adds no inline latency and is not a single point of failure.

---

**Q35. Correct: A**

Security Health Analytics custom modules evaluate resource configuration (via Cloud Asset Inventory) using CEL expressions, which fits checking a missing label. Event Threat Detection custom modules analyze log streams using YARA-L rules, which fits detecting anomalous behavioral patterns in Cloud Audit Logs.

---

**Q36. Correct: C**

Secret Manager fully supports Cloud Audit Logs, but Data Access logs (ADMIN_READ, DATA_READ, DATA_WRITE) are disabled by default for nearly all services, with BigQuery as the notable exception. They must be explicitly enabled via an Audit Config at the project or organization level to capture successful secret read access, including access by admins/owners.

---

**Q37. Correct: A**

VM Manager's OS Patch Management lets administrators define patch deployment schedules (including urgency-based windows suitable for a 48-hour SLA) and provides Patch Compliance dashboards/reports showing per-instance patch state, directly satisfying both automated patching and reporting requirements.

---

**Q38. Correct: A**

Artifact Registry vulnerability scanning produces the CVE results, and a custom Binary Authorization attestor lets the CI pipeline programmatically create an attestation only when the policy (e.g., no CRITICAL/HIGH CVEs) is satisfied. Binary Authorization then blocks deployment to GKE unless that attestation is present, giving a true preventive, policy-enforced gate.

---

**Q39. Correct: D**

Cloud IDS is a fully managed intrusion detection service that mirrors traffic internally and applies signature-based threat detection (built on Palo Alto Networks technology) for things like port scans, malware, and C2 traffic, without requiring the customer to deploy or operate any inspection appliance.

---

**Q40. Correct: C**

Admin Activity, System Event, and Policy Denied audit logs are always enabled and cannot be turned off. Data Access audit logs, however, are disabled by default for every service except BigQuery (which is enabled by default and cannot be disabled) — for Cloud Storage and most other services, Data Access logs must be explicitly enabled via an IAM Audit Config entry in the project or organization IAM policy before they are even generated, so no sink filter can capture logs that were never produced.

---

**Q41. Correct: B**

Assured Workloads control packages (e.g., FedRAMP High) bundle data residency enforcement, personnel access restrictions (support limited to US persons), and required org policy/encryption baselines into one managed compliance regime specifically designed for this scenario.

---

**Q42. Correct: C**

Access Approval requires explicit customer approval before Google personnel can access customer content, and it technically blocks access if the request is denied — matching the enforcement requirement in the scenario.

---

**Q43. Correct: A**

Setting constraints/gcp.resourceLocations at the organization node enforces the region restriction across all current and future projects via policy inheritance, with no requirement to manage personnel restrictions or per-resource manual configuration.

---

**Q44. Correct: D**

Under the shared responsibility model, CMEK gives the customer control over key lifecycle (rotation, IAM permissions, disabling/destroying) while Google continues to manage the underlying storage encryption infrastructure and hardware — a common exam distinction on the boundary of CMEK responsibility.

---

**Q45. Correct: B**

Data Access audit logs (specifically DATA_READ events) are required to capture read access to BigQuery table data, and exporting them to a locked (retention-policy-protected, non-deletable) Cloud Storage bucket satisfies both the tamper-resistance and long-term retention requirements.

---

