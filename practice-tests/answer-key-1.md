# Answer Key — Practice Test 1

**Q1. Correct: D**

Workload Identity Federation lets external workloads (like GitHub Actions) exchange a short-lived OIDC token for temporary Google credentials via service account impersonation, eliminating the need for any downloaded key.

---

**Q2. Correct: D**

IAM deny policies are evaluated together with allow policies at every resource level; an explicit deny always wins over any allow grant, including permissions held by highly privileged roles like Owner.

---

**Q3. Correct: D**

Privileged Access Manager is purpose-built for just-in-time privileged access: it supports requestor/approver workflows, automatically expiring grants, and detailed audit logging of the entire request lifecycle.

---

**Q4. Correct: A**

Workforce Identity Federation allows external workforce users authenticated by a third-party SAML or OIDC identity provider to access Google Cloud resources and the console directly, without needing a Cloud Identity or Workspace account per user.

---

**Q5. Correct: A**

The constraints/iam.disableServiceAccountKeyCreation boolean constraint, when enforced, blocks the creation of new user-managed service account keys across the org, folder, or project it is applied to.

---

**Q6. Correct: A**

Hardware security keys using the FIDO U2F/WebAuthn standard (such as Titan Security Keys) are phishing-resistant because the cryptographic challenge is bound to the origin, making them Google's recommended strongest 2SV method for super admin accounts.

---

**Q7. Correct: C**

Workload Identity Federation lets GitHub Actions exchange a short-lived OIDC token for a Google Cloud access token via service account impersonation, so no service account key ever exists.

---

**Q8. Correct: C**

Privileged Access Manager is purpose-built for just-in-time privilege elevation: it defines entitlements, requires approval workflows, enforces a maximum duration, and automatically revokes access, with full audit trail.

---

**Q9. Correct: C**

IAM deny policies are explicit denies that are evaluated ahead of allow policies in the IAM authorization check; an explicit deny cannot be overridden by any allow grant, including highly privileged roles like Owner.

---

**Q10. Correct: A**

iam.disableServiceAccountKeyCreation blocks the creation of new user-managed service account keys organization-wide while leaving already-existing keys valid, exactly matching the stated requirement.

---

**Q11. Correct: D**

An Access Level is the named, reusable Access Context Manager object that encapsulates conditions like IP range, device policy, and region; perimeters and ingress/egress rules reference Access Levels to make access decisions.

---

**Q12. Correct: A**

Shared VPC lets a host project centrally own the network topology and firewall rules while service projects attach Compute Engine, GKE, or Cloud SQL resources to shared subnets, giving the security team centralized network governance without limiting per-team resource management.

---

**Q13. Correct: D**

Private Google Access is a subnet-level setting that allows VMs without external IP addresses to reach Google APIs and services using internal IP addresses, exactly matching the described requirement.

---

**Q14. Correct: C**

Private Service Connect lets the provider publish a service via a service attachment; consumers create endpoints with private IPs in their own VPCs, avoiding IP overlap conflicts and without exposing the provider's full network topology, which is the standard pattern for multi-tenant SaaS consumption.

---

**Q15. Correct: D**

Access Levels evaluate context such as source IP address ranges (and optionally device policy) and can be attached to a perimeter to conditionally permit identities from the corporate network's known public IP ranges, without opening access to everyone else.

---

**Q16. Correct: A**

Hierarchical firewall policies attached at the organization or folder level are evaluated before project-level VPC firewall rules and can be configured so that lower-level rules cannot override them, enforcing a mandatory rule consistently across all VPCs in scope.

---

**Q17. Correct: D**

Secure Web Proxy is a fully managed Google Cloud service that provides Layer 7, URL-based filtering of outbound web traffic from VMs, allowing approved destinations while blocking others, without requiring self-managed proxy infrastructure.

---

**Q18. Correct: D**

A Private Service Connect endpoint for Google APIs creates an internal IP address in the VPC that can be reached both by VMs inside the VPC and, via route advertisement over VPN or Interconnect, by on-premises hosts — giving both populations private access to Google APIs.

---

**Q19. Correct: D**

Cloud NAT provides managed, outbound-only network address translation for instances without external IP addresses. It allows VMs to initiate connections to the internet (e.g., to download updates) while blocking any unsolicited inbound connection attempts, since NAT only allows return traffic for connections the instance initiated.

---

**Q20. Correct: C**

Access Context Manager access levels can define conditions such as an allowed IP subnetwork. Attaching this access level to the perimeter allows requests that satisfy both a valid IAM identity and the specified source IP range to pass through the perimeter boundary, which is exactly the requirement here.

---

**Q21. Correct: B**

HA VPN uses IPsec tunnels, which encrypt traffic by default, and when deployed with two tunnels on two separate external interfaces (the standard HA VPN topology), Google publishes a 99.99% availability SLA for the gateway.

---

**Q22. Correct: A**

FPE is deterministic (same input always produces the same token) and preserves the original format/length, so analysts can still join on the tokenized value while the real card number is never exposed.

---

**Q23. Correct: A**

Cloud EKM lets you keep key material in a supported external key management partner system outside Google's infrastructure. Google calls out to the external system for every cryptographic operation, allowing the external system to log and audit access independently of Google.

---

**Q24. Correct: A**

Cloud KMS enforces a minimum rotation period of 24 hours for automatically rotated keys; you cannot configure a shorter interval such as hourly rotation.

---

**Q25. Correct: C**

roles/secretmanager.secretAccessor grants only the secretmanager.versions.access permission, allowing the caller to retrieve secret payload data without permission to list secrets, view metadata, or manage versions/IAM policies — the least-privilege fit for this use case.

---

**Q26. Correct: B**

Confidential Computing addresses the 'data in use' state by using hardware-based memory encryption (e.g., AMD SEV) so that data being actively processed in memory remains encrypted and inaccessible even to the underlying hypervisor or Google infrastructure operators.

---

**Q27. Correct: C**

VPC Service Controls establishes a service perimeter that blocks data exfiltration to resources outside the perimeter even if credentials are compromised, while Private Google Access/Private Service Connect ensures API traffic to Vertex AI/Gemini stays on Google's private network rather than the public internet — together satisfying both requirements.

---

**Q28. Correct: D**

Format-preserving encryption in Sensitive Data Protection produces a deterministic token that keeps the same format as the original value (so joins/analytics still work) and can be cryptographically reversed by authorized users holding the wrapped KMS key, satisfying both the analytics and re-identification requirements.

---

**Q29. Correct: B**

Cloud EKM lets a customer manage keys in a supported external key management partner system (on their own HSM or third-party service) while Google Cloud services call out over the network to that external system for each encrypt/decrypt operation, keeping key material physically outside Google infrastructure.

---

**Q30. Correct: B**

Cloud KMS supports a built-in rotation_period and next_rotation_time property on a CryptoKey; when set, Cloud KMS automatically generates a new primary key version at the configured interval with zero manual scripting or scheduling required.

---

**Q31. Correct: B**

Confidential Computing uses hardware-based memory encryption to protect data in use, ensuring genomic data remains encrypted while being processed in memory during model training, which directly addresses protection against hypervisor or infrastructure-level compromise.

---

**Q32. Correct: B**

Binary Authorization enforces admission control at the cluster level (via a webhook), so it blocks any deployment path -- CI/CD or manual kubectl -- unless the image carries a valid attestation from the required attestor, giving true deploy-time enforcement.

---

**Q33. Correct: B**

Cloud IDS is a fully managed service (built on Palo Alto Networks technology) that mirrors and inspects packet payloads for known threat signatures, correlating findings and reporting them, without requiring the customer to operate any third-party appliance.

---

**Q34. Correct: B**

The VPC Flow Logs sample rate ranges from 0.0 to 1.0, where 1.0 means 100% of flows are logged (no sampling). This is required to satisfy a 'log every flow' compliance mandate.

---

**Q35. Correct: C**

An aggregated sink created at the organization resource with include-children enabled automatically captures matching log entries from all current and future projects/folders beneath it and routes them to a single destination such as BigQuery, with no per-project configuration needed.

---

**Q36. Correct: A**

Security Health Analytics is a built-in SCC service that continuously scans resource configurations against a set of predefined detectors covering misconfigurations like public buckets, overly permissive firewall rules, and weak IAM bindings, requiring no custom rule authoring.

---

**Q37. Correct: D**

Admin Activity audit logs capture calls that write or change resource configuration or metadata (e.g., creating a VM, changing an IAM policy). They are always written and cannot be disabled or excluded.

---

**Q38. Correct: C**

Binary Authorization lets you require cryptographically signed attestations tied to specific pipeline stages (e.g., 'vulnerability scan passed') and enforces this at deploy time via an admission controller, blocking any image lacking the required attestation.

---

**Q39. Correct: B**

Cloud IDS uses Packet Mirroring under the hood to send a copy of traffic to a managed detection engine (built on Palo Alto Networks technology) that performs signature-based and protocol-anomaly threat detection — exactly the managed capability requested.

---

**Q40. Correct: A**

An aggregated sink created at the organization (or folder) level with includeChildren=true automatically captures matching logs — including Data Access audit logs via a logName filter — from all existing and future descendant projects, routing them to one destination without per-project configuration.

---

**Q41. Correct: A**

Access Approval requires Google support staff to obtain explicit customer approval before accessing customer content, satisfying a 'pre-authorization required' regulatory control.

---

**Q42. Correct: B**

Assured Workloads is purpose-built to enforce a bundle of controls — data residency, personnel access restrictions, and control mapping to specific compliance regimes (FedRAMP, IL4, EU regions, etc.) — in a single managed folder.

---

**Q43. Correct: B**

constraints/gcp.resourceLocations restricts the physical locations where new resources can be created across supported services, directly enforcing data residency requirements.

---

**Q44. Correct: C**

Regardless of the compute abstraction level used, the customer always owns data classification and access policy decisions — Google never determines what data is sensitive or who should access it.

---

**Q45. Correct: C**

PCI DSS scoping is based on the actual data flow: components that store, process, or transmit cardholder data — and anything connected to those components without adequate segmentation — are in scope. The isolated wiki cluster with no connectivity to the cardholder data environment is correctly excluded.

---

