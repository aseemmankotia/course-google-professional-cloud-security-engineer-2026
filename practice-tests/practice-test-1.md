# Practice Test 1: Google Cloud Security Engineer Exam Prep

- **Questions:** 45
- **Time limit:** 90 minutes
- **Passing score:** 70%%

> Answers are in `answer-key-1.md`. Track results in `score-tracker.md`.

---

**Q1.** A company's CI/CD pipeline runs on GitHub Actions and needs to deploy resources to a Google Cloud project. Security policy prohibits storing any long-lived credentials in the pipeline configuration. Which solution BEST meets this requirement?

A) Create a service account key, base64-encode it, and store it as a GitHub Actions encrypted secret
B) Configure Workforce Identity Federation so the pipeline authenticates as a workforce identity via SAML
C) Grant the GitHub organization the Editor role at the project level using a shared Google account
D) Configure Workload Identity Federation with GitHub's OIDC provider mapped to a workload identity pool that impersonates a Google Cloud service account

---

**Q2.** Which statement correctly describes how IAM deny policies interact with allow policies in Google Cloud?

A) Allow policies granted to organization administrators always override any deny policy
B) Deny policies apply only to service accounts and cannot restrict human user principals
C) Deny policies require the caller to hold the Security Admin role before they take effect
D) Deny policies are evaluated alongside allow policies, and an explicit deny always overrides any allow grant, regardless of role

---

**Q3.** An on-call engineer needs elevated access to a production project for exactly 4 hours to troubleshoot an incident. The security team requires an approval workflow, automatic expiration of the grant, and a full audit trail of the request and usage. Which Google Cloud capability BEST satisfies these requirements?

A) Access Context Manager with a device-based access level applied to the project
B) Grant the engineer the Owner role manually and remove it after the incident is resolved
C) Service account impersonation configured with an IAM condition restricting the resource type
D) Privileged Access Manager (PAM) with a time-bound entitlement requiring approval

---

**Q4.** A company wants its employees to sign in to the Google Cloud console using their existing on-premises Active Directory credentials through the corporate SAML identity provider, without provisioning a Cloud Identity or Workspace account for every employee. Which feature should be configured?

A) Workforce Identity Federation
B) Google Cloud Directory Sync (GCDS)
C) Workload Identity Federation
D) Identity-Aware Proxy (IAP)

---

**Q5.** A security team wants to enforce an organization-wide policy that prevents any user or service account from creating new service account keys. Which organization policy constraint should be enforced?

A) constraints/iam.disableServiceAccountKeyCreation
B) constraints/iam.automaticIamGrantsForDefaultServiceAccounts
C) constraints/compute.disableSerialPortAccess
D) constraints/iam.allowServiceAccountCredentialLifetimeExtension

---

**Q6.** A security team wants to enforce the strongest, phishing-resistant 2-Step Verification method for all Cloud Identity super admin accounts. Which method should be required?

A) Titan Security Keys or other FIDO U2F/WebAuthn hardware security keys
B) Google prompt sent to a registered mobile device
C) SMS verification codes sent to a registered phone number
D) Printed backup codes stored in a secure location

---

**Q7.** A company uses GitHub Actions to deploy infrastructure to Google Cloud. The security team requires that no long-lived service account keys be stored in GitHub. Which configuration BEST meets this requirement?

A) Create a service account key, encrypt it with Cloud KMS, and store the encrypted blob as a GitHub secret
B) Create a service account, download a JSON key, and rotate it automatically every 90 days
C) Configure Workload Identity Federation with a workload identity pool and provider that trusts GitHub's OIDC tokens, and grant the mapped identity permission to impersonate a service account
D) Use the project's default compute service account with domain-wide delegation enabled

---

**Q8.** A financial services company wants engineers to hold no standing IAM permissions on production projects, but be able to request temporary elevated access (for example roles/compute.admin) for a fixed duration with manager approval and full audit logging. Which Google Cloud capability should they use?

A) Organization Policy constraint that blocks all IAM role bindings by default
B) Policy Intelligence Recommender to suggest role downgrades after access is granted
C) Privileged Access Manager (PAM) to create an entitlement that grants time-bound, approved access
D) IAM Conditions attached directly to a permanent role binding to add a time-based expiry

---

**Q9.** An organization has an IAM deny policy on a folder that denies iam.serviceAccounts.actAs to all principals for resources tagged env=prod, with no exceptions defined. A project admin has been granted roles/owner on a project under that folder. What happens when the admin attempts to impersonate a service account on a prod-tagged resource in that project?

A) The action is allowed because roles/owner always overrides deny policies
B) The action is denied only if Access Context Manager is also configured for the resource
C) The action is denied because deny policies are evaluated before allow policies and always take precedence
D) The action is allowed only if the admin also has a matching allow IAM condition for env=prod

---

**Q10.** A security team wants to prevent any user in the organization from creating new service account keys, while allowing existing key-based workflows to continue functioning until they are migrated to Workload Identity Federation. Which organization policy constraint should they enforce at the organization node?

A) iam.disableServiceAccountKeyCreation
B) constraints/iam.allowedPolicyMemberDomains
C) iam.automaticIamGrantsForDefaultServiceAccounts
D) iam.disableServiceAccountKeyUpload

---

**Q11.** When configuring VPC Service Controls to restrict access based on device trust and source IP address, which Access Context Manager object defines the reusable set of attributes (such as IP ranges, device policy, and geographic region) that a service perimeter's ingress and egress rules reference?

A) VPC Firewall Policy Rule
B) Service Perimeter Bridge
C) Access Binding
D) Access Level

---

**Q12.** A company has multiple GCP projects for different application teams (finance, HR, and marketing). The security team wants centralized control over network resources and firewall rules, while allowing each application team to independently manage resources such as Compute Engine instances and Cloud SQL databases in their own projects. Which networking approach BEST meets this requirement?

A) Use Shared VPC with the security team's project as the host project and the application team projects as service projects
B) Configure VPC Peering between each project's VPC network
C) Deploy VPC Service Controls to create a service perimeter around all projects
D) Create separate standalone VPC networks in each project and connect them using Cloud VPN

---

**Q13.** An organization runs Compute Engine VMs that must call Cloud Storage and BigQuery APIs. For security reasons these VMs must not have external IP addresses, but connectivity to Google APIs must continue to work. Which feature should be enabled on the VM's subnet to satisfy this requirement?

A) Cloud NAT
B) Private Service Connect for Google APIs
C) A VPC Service Controls perimeter
D) Private Google Access

---

**Q14.** A SaaS company hosts a proprietary service in its own VPC network and needs to let multiple customer VPC networks in different organizations privately consume this service. The company wants to avoid overlapping IP address conflicts and must not expose its entire VPC network topology to customers. Which solution BEST meets these requirements?

A) Configure VPC Peering between the SaaS company's VPC and each customer VPC
B) Configure Shared VPC and add each customer's project as a service project
C) Publish the service using Private Service Connect with a service attachment, allowing customers to create PSC endpoints
D) Establish HA VPN tunnels between the SaaS company's network and each customer network

---

**Q15.** A financial services company has a VPC Service Controls perimeter around projects containing sensitive Cloud Storage and BigQuery data. Employees connecting from the corporate on-premises network, which has no VPN or Interconnect connection to Google Cloud, need to access the protected resources using their Google identity credentials from known public egress IP ranges. Which configuration should the security team implement without weakening the perimeter for all other traffic?

A) Configure a perimeter bridge between the on-premises network and the protected perimeter
B) Configure an ingress rule allowing all traffic sourced from RFC 1918 private address ranges
C) Temporarily disable VPC Service Controls during business hours
D) Create an Access Level based on the on-premises network's known public IP address ranges and attach it to the perimeter

---

**Q16.** A multinational corporation wants to enforce a mandatory rule blocking SSH access (port 22) from the internet across all VPC networks in every project within a specific folder, and project owners must not be able to override this rule. Which Google Cloud NGFW feature should be used?

A) A hierarchical firewall policy applied at the folder level with rules that cannot be overridden
B) VPC firewall rules configured with a priority value of 65534 in each project
C) A Cloud Armor security policy with a preconfigured WAF rule
D) Firewall Insights recommendations applied per project

---

**Q17.** A company runs Compute Engine VMs in a VPC that need outbound internet access only to specific approved SaaS websites, such as approved software update servers, while all other external websites must be blocked based on URL, and the company does not want to deploy or manage a third-party proxy appliance. Which Google Cloud service should be used?

A) Cloud NAT with a defined static IP range
B) Cloud Armor with an edge security policy
C) Private Service Connect for Google APIs
D) Secure Web Proxy

---

**Q18.** A company has Compute Engine VMs without external IP addresses that must reach Google APIs such as Cloud Storage. The company also has an on-premises network connected via Cloud VPN, and administrators there also need private access to the same Google APIs without traversing the public internet. Which solution BEST meets both requirements?

A) Enable Private Google Access on the VPC subnet
B) Configure Cloud NAT on the VPC to allow outbound internet access
C) Deploy a Serverless VPC Access connector and route on-premises traffic through it
D) Create a Private Service Connect endpoint for Google APIs in the VPC and advertise the route to the on-premises network over the VPN

---

**Q19.** Compute Engine VM instances in a VPC have no external IP addresses. They need to download OS patches from the public internet but must never accept unsolicited inbound connections initiated from the internet. Which service should be configured to meet this requirement?

A) Private Google Access
B) An external HTTP(S) Load Balancer
C) Cloud Interconnect
D) Cloud NAT

---

**Q20.** Your organization protects a BigQuery service perimeter with VPC Service Controls. External auditors, who authenticate with their own Google identities from a known, fixed corporate IP range, must be granted read access to specific datasets inside the perimeter. Which configuration should you implement?

A) Add an ingress rule to the perimeter that allows ANY_IDENTITY from the auditors' project
B) Add an egress rule to the perimeter that permits traffic to the auditors' external IP addresses
C) Create an Access Context Manager access level with an IP subnetwork condition matching the auditors' IP range, and attach it to the perimeter's access levels
D) Disable VPC Service Controls enforcement on the BigQuery API for the perimeter

---

**Q21.** A security team must select a hybrid connectivity option to link an on-premises data center to a VPC. The option must provide a 99.99% availability SLA and encrypt data in transit by default without requiring any additional encryption configuration. Which option BEST meets these requirements?

A) Dedicated Interconnect
B) HA VPN configured with two tunnels through a Cloud Router
C) Partner Interconnect
D) Cloud CDN

---

**Q22.** A healthcare analytics team stores credit card numbers in BigQuery. Analysts must be able to JOIN records across multiple tables using the card number as a key, but the actual card numbers must never be visible to analysts, and the tokenized value must retain the same 16-digit numeric format so downstream validation logic still works. Which Sensitive Data Protection technique should they apply?

A) Apply format-preserving encryption (FPE) using Sensitive Data Protection's crypto-based tokenization
B) Encrypt the column using Cloud KMS envelope encryption with a randomly generated IV per record
C) Apply DLP redaction to replace each digit with an asterisk
D) Enable customer-managed encryption keys (CMEK) on the BigQuery dataset

---

**Q23.** A financial services customer's compliance team requires that the actual key material used to encrypt their Cloud Storage data never resides within Google's infrastructure at any point, and that key access requests can be logged and audited by an external, non-Google system. Which Google Cloud capability satisfies this requirement?

A) Cloud External Key Manager (Cloud EKM)
B) Cloud HSM
C) Cloud KMS software-backed keys
D) Confidential Computing

---

**Q24.** You are configuring an automatic rotation schedule for a CMEK key in Cloud KMS that protects a Cloud Storage bucket. What is the minimum rotation period Cloud KMS allows you to set for automatic key rotation?

A) 24 hours (1 day)
B) 1 hour
C) 7 days
D) 90 days

---

**Q25.** A CI/CD pipeline's service account needs to retrieve the payload of specific secret versions in Secret Manager during deployment, but it must not be able to list all secrets in the project, view secret metadata/IAM policies, or create new secret versions. Which IAM role should you grant to this service account?

A) roles/secretmanager.admin
B) roles/secretmanager.viewer
C) roles/secretmanager.secretAccessor
D) roles/owner

---

**Q26.** Which statement best describes what Confidential Computing (e.g., Confidential VMs) protects against on Google Cloud?

A) It encrypts data at rest on persistent disks using customer-managed encryption keys
B) It protects data in use by keeping memory contents encrypted with hardware-based keys during processing, even from the hypervisor and Google's own operators
C) It encrypts data in transit between the VM and other services using TLS
D) It restricts data exfiltration by enforcing a VPC Service Controls security perimeter around the VM's project

---

**Q27.** A company builds a custom generative AI application on Vertex AI that uses Gemini models to process prompts and responses containing sensitive customer PII. Security requires that this sensitive data never traverses the public internet and cannot be exfiltrated to an unauthorized Google Cloud project, even by a compromised service account with valid API credentials. Which combination of controls BEST meets this requirement?

A) Enable customer-managed encryption keys (CMEK) on Vertex AI resources only
B) Rely on Google's default encryption at rest for all training data and model artifacts
C) Configure a VPC Service Controls perimeter around the Vertex AI project and require API access via Private Google Access or Private Service Connect
D) Store the Gemini API keys in Secret Manager and rotate them regularly

---

**Q28.** A retail company stores customer credit card numbers in a BigQuery table used for fraud analytics. Analysts must be able to run queries that reference the same card across transactions, and a small fraud investigation team must occasionally recover the original card number. Which Sensitive Data Protection (Cloud DLP) transformation should be applied to the column?

A) Apply redaction to permanently remove the card number values
B) Apply generalization (bucketing) to replace card numbers with numeric ranges
C) Apply SHA-256 hashing of the card number before loading into BigQuery
D) Apply format-preserving encryption (FPE) using a wrapped Cloud KMS key so the token is deterministic and reversible

---

**Q29.** A financial services customer requires that their Cloud KMS-protected encryption keys physically reside on hardware security modules under their own operational control, outside of Google's infrastructure, with Google required to call out to that external system for every cryptographic operation. Which Google Cloud capability satisfies this requirement?

A) Customer-managed encryption keys (CMEK) stored in Cloud KMS
B) Cloud External Key Manager (Cloud EKM)
C) Confidential Computing with Confidential VMs
D) Customer-supplied encryption keys (CSEK) passed at request time

---

**Q30.** You manage a Cloud KMS CryptoKey used to protect a Cloud Storage bucket via CMEK. Compliance requires that new key versions be generated automatically every 90 days without any manual intervention or scripts. Which configuration should you apply?

A) Create a Cloud Scheduler job that runs gcloud kms keys versions create every 90 days
B) Set the CryptoKey's rotation_period to 7776000 seconds and configure a next_rotation_time
C) Set the CryptoKeyVersion state to DESTROY_SCHEDULED after 90 days
D) Enable object versioning on the Cloud Storage bucket with a 90-day lifecycle rule

---

**Q31.** A healthcare research team trains a custom Vertex AI model on sensitive genomic data. Their compliance framework requires the data remain encrypted even while it is being actively processed in memory during training, protecting it against a compromise of the underlying hypervisor or a malicious insider with infrastructure-level access. Which capability should they enable for the training job?

A) Customer-managed encryption keys (CMEK) on the Vertex AI training pipeline
B) Confidential Computing (Confidential VMs / Confidential GKE Nodes) for the training workload
C) VPC Service Controls perimeter around the Vertex AI project
D) Sensitive Data Protection de-identification of the training dataset before training

---

**Q32.** A company's CI/CD pipeline deploys container images to a GKE cluster running production payment workloads. The security team wants to guarantee that only images which have passed a CVE vulnerability scan and been explicitly approved can ever be deployed to that cluster, even if a developer tries to deploy directly with kubectl. Which solution BEST meets this requirement?

A) Use the Container Analysis API scan results directly to block the deployment in the CI/CD tool
B) Configure a Binary Authorization policy on the cluster requiring a valid attestation signed by a vulnerability-scanning attestor
C) Restrict which container registries can be used via an Organization Policy constraint
D) Enable a VPC Service Controls perimeter around the GKE cluster's project

---

**Q33.** A security team needs to detect network-based threats such as malware command-and-control traffic and port scans by inspecting packet payloads traversing a VPC, without deploying, licensing, or managing third-party IDS software themselves. Which Google Cloud service should they use?

A) VPC Flow Logs
B) Cloud IDS
C) Cloud Armor
D) Packet Mirroring

---

**Q34.** A compliance requirement for a PCI-DSS workload states that VPC Flow Logs must capture every single flow on the subnet -- no sampling is permitted. Which flow log configuration value must be set to satisfy this requirement?

A) Sample rate of 0.0
B) Sample rate of 1.0
C) Sample rate of 0.5
D) Sample rate of 0.1

---

**Q35.** A company manages several hundred GCP projects organized under multiple folders. The security team wants to collect Admin Activity and Data Access audit logs from every project, current and future, into a single centralized BigQuery dataset for analysis, without configuring an export sink in each project individually. What should they configure?

A) Enable Data Access audit logs at the organization Org Policy level only
B) Configure a log sink in each project's _Default log bucket that exports to BigQuery
C) Create an aggregated log sink at the organization node with include-children enabled, with the destination set to a BigQuery dataset
D) Schedule a recurring Cloud Logging export job using Cloud Scheduler

---

**Q36.** A security team wants Security Command Center to automatically flag common misconfigurations, such as a Cloud Storage bucket with public access or a firewall rule allowing ingress from 0.0.0.0/0, without writing any custom detection rules. Which SCC feature provides this out-of-the-box?

A) Security Health Analytics
B) Event Threat Detection
C) Web Security Scanner
D) Container Threat Detection

---

**Q37.** Which Cloud Audit Logs log type records API calls that create, modify, or delete the configuration or metadata of resources, and is always enabled for every service with no option to disable it?

A) System Event logs
B) Policy Denied logs
C) Data Access logs
D) Admin Activity logs

---

**Q38.** A company deploys containerized workloads to GKE and requires that only images which passed a vulnerability scan during the CI/CD pipeline can be deployed to production clusters. Which solution BEST enforces this requirement?

A) Configure Cloud IDS to inspect container images before they are pulled by GKE nodes
B) Rely on Container Analysis vulnerability scan results without configuring any enforcement policy
C) Enable Binary Authorization with an attestor that requires a signed attestation created only after the vulnerability scanning step passes, and enforce the policy on the cluster
D) Restrict the GKE cluster's service account IAM roles to read-only on Artifact Registry

---

**Q39.** A security team wants to detect known attack signatures (e.g., exploit patterns, command-and-control traffic) crossing a VPC boundary, using a fully managed service rather than building custom signature-matching logic.

A) Enable VPC Flow Logs and build custom BigQuery queries to detect anomalies
B) Enable Cloud IDS, which mirrors traffic to a managed intrusion detection engine that inspects packet payloads against known threat signatures
C) Configure Packet Mirroring to a self-managed VM with no IDS software installed
D) Enable Data Access audit logs on all VPC-related resources

---

**Q40.** An organization wants all Data Access audit logs from every current and future project under the organization node centralized into a single BigQuery dataset for security analysis, without configuring exports in each project individually.

A) Create an aggregated log sink at the organization resource with includeChildren set to true, filtered for data access audit logs, exporting to the BigQuery dataset
B) Create a log sink in each existing project pointing to the shared BigQuery dataset
C) Enable Access Transparency logs and export them using Cloud Storage lifecycle rules
D) Use Security Command Center Premium's asset inventory export feature to collect audit logs

---

**Q41.** A financial services company operating on Google Cloud must comply with a regulation stating that Google support personnel must not access the customer's data without the customer explicitly authorizing each specific access request in advance. Which Google Cloud capability satisfies this requirement?

A) Access Approval
B) Cloud Audit Logs Data Access logs
C) VPC Service Controls perimeter
D) Access Transparency

---

**Q42.** A healthcare company must ensure that customer data is processed and stored only within a specific region, that support access is restricted to personnel meeting defined criteria, and that compliance controls map to a named regulatory framework such as FedRAMP or IL4. Which Google Cloud product should they provision?

A) Organization Policy Service with a resource location constraint only
B) Assured Workloads
C) VPC Service Controls
D) Cloud KMS with customer-managed encryption keys

---

**Q43.** A regulation requires that all new Google Cloud resources be created only within an approved geographic region to satisfy data residency obligations. Which organization policy constraint should be applied at the organization or folder level to enforce this?

A) constraints/iam.allowedPolicyMemberDomains
B) constraints/gcp.resourceLocations
C) constraints/sql.restrictAuthorizedNetworks
D) constraints/compute.trustedImageProjects

---

**Q44.** Under Google Cloud's shared responsibility model, which of the following remains the customer's responsibility regardless of whether they deploy on Compute Engine, GKE, or Cloud Run?

A) Patching the underlying hypervisor
B) Physical security of the data center housing the hardware
C) Classifying data and defining IAM access policies for that data
D) Maintaining the physical network backbone between regions

---

**Q45.** A company processes credit card payments using Compute Engine VMs, Cloud Storage buckets, and BigQuery datasets that store cardholder data. It also runs a separate GKE cluster for an internal wiki tool with no network path or data flow to the payment systems. When scoping the environment for PCI DSS assessment, which approach is correct?

A) Scope is determined only by which resources carry a 'pci' label, regardless of data flow
B) All resources in the same Google Cloud project are automatically in scope
C) Only systems that store, process, or transmit cardholder data, plus any systems connected to them, are in scope
D) The GKE wiki cluster must be included in scope because it resides in the same organization

---

