# Practice Test 2: Google Cloud Security Engineer Exam Prep

- **Questions:** 45
- **Time limit:** 90 minutes
- **Passing score:** 70%%

> Answers are in `answer-key-2.md`. Track results in `score-tracker.md`.

---

**Q1.** A security engineer at a media company must ensure that a group of external contractors (group: contractors@example.com) can never delete Compute Engine instances, even though this group inherits the Editor role from a legacy binding at the organization level that other teams still depend on. The engineer cannot modify the existing Editor binding because it is required for other principals. Which approach enforces this restriction with the least disruption?

A) Create an organization policy constraint restricting compute.instances.delete for the organization
B) Remove the Editor role from the organization-level binding and replace it with a custom role for all affected principals
C) Create an IAM deny policy at the folder containing the contractors' projects, denying compute.instances.delete permission to the principal set contractors@example.com
D) Add a VPC Service Controls perimeter around the contractors' projects to block delete operations

---

**Q2.** A workforce is federated into Google Cloud using Workforce Identity Federation with Okta as the external IdP via SAML. The security team must ensure that only users whose Okta SAML assertion contains the attribute department with value finance can assume access through the workforce pool provider — all other authenticated Okta users should be denied federation entirely. What should they configure?

A) An IAM deny policy on the workforce pool denying access to principals without the finance attribute
B) An organization policy constraint restricting Workforce Identity Federation to specific SAML issuers
C) A VPC Service Controls access level scoped to the finance department
D) An attribute mapping for google.subject combined with an attribute condition on the workforce pool provider that evaluates assertion.attributes.department == 'finance'

---

**Q3.** An organization wants engineers to normally hold no standing IAM roles above Viewer on production projects, but occasionally need temporary elevated access (e.g., Editor) for incident response, with a mandatory approval step from a designated approver and automatic revocation after a fixed duration. Which Google Cloud capability should be used to satisfy this requirement?

A) IAM Conditions with a date/time expression on the Editor role binding
B) Policy Intelligence IAM Recommender to periodically right-size roles
C) Access Context Manager access levels tied to a break-glass service account
D) Privileged Access Manager (PAM) entitlements configured with an approval workflow and a maximum grant duration

---

**Q4.** A DevOps team runs CI/CD pipelines in GitHub Actions that must deploy to Google Cloud. Security policy prohibits the use of any downloaded service account key files, and access must be restricted so that only workflows running from the repository org/repo-name on the main branch can obtain Google Cloud credentials. What should be configured?

A) A service account key stored as an encrypted GitHub Actions secret, rotated every 90 days
B) A Workforce Identity Federation pool configured for GitHub as an external workforce IdP
C) An organization policy constraint disabling service account key creation, with a manually issued long-lived OAuth token for GitHub Actions
D) A workload identity pool and provider configured for GitHub's OIDC issuer, with an attribute condition restricting assertion.repository and assertion.ref to the specific repo and branch, mapped to a service account via impersonation

---

**Q5.** A company enforces VPC Service Controls around a perimeter containing BigQuery datasets with sensitive financial data. Analysts must be allowed to query these datasets only when connecting from company-managed, encrypted laptops on the corporate IP range; connections from personal devices or external IPs must be blocked even if the analyst has valid IAM permissions. What should be configured?

A) An IAM Conditions expression on each analyst's BigQuery role binding checking request.time
B) An Access Context Manager access level requiring the corporate IP range and a device policy (encryption + corporate ownership), attached as an ingress rule on the service perimeter
C) An organization policy constraint restricting BigQuery access to a single VPC network
D) A Cloud NGFW firewall rule blocking BigQuery API traffic from non-corporate IP ranges

---

**Q6.** An organization's audit found that several teams had created user-managed service account keys and stored them insecurely, leading to a credential leak. The CISO wants to prevent any new service account key creation across the entire organization going forward, but one legacy on-premises system in project legacy-sys-prod still requires a key temporarily until it can be migrated to Workload Identity Federation next quarter. What is the best way to enforce this?

A) Manually delete all existing service account keys and instruct teams not to create new ones via an internal policy document
B) Enable the organization policy constraint only on the folder containing legacy-sys-prod
C) Create an IAM deny policy denying iam.serviceAccounts.keys.create at the organization level with no exceptions
D) Set the organization policy constraint constraints/iam.disableServiceAccountKeyCreation at the organization node, then set an override for project legacy-sys-prod allowing key creation

---

**Q7.** A security team must guarantee that no principal in the organization — regardless of any IAM allow bindings granted at project, folder, or organization level — can invoke compute.instances.delete, except for members of a designated 'break-glass' group who need this capability during incident response. Which approach satisfies this requirement?

A) Create an IAM deny policy at the organization node denying compute.instances.delete to allUsers, with a deny exception for principals in the break-glass group.
B) Create an organization policy constraint restricting compute.instances.delete permission grants.
C) Use VPC Service Controls to block calls to the Compute Engine delete API method.
D) Remove the compute.instances.delete permission from all custom IAM roles defined in the organization.

---

**Q8.** Workloads running on AWS EC2 instances need to access Cloud Storage buckets without using long-lived service account keys. The security team wants to map the AWS caller's role ARN to the Google identity and restrict federation to a single AWS account and IAM role. Which configuration meets this requirement?

A) Create a service account key, store it in AWS Secrets Manager, and use it to authenticate to Google Cloud APIs.
B) Configure Workforce Identity Federation with a SAML provider pointing to AWS IAM Identity Center.
C) Create a Workload Identity Pool with an AWS provider, map google.subject to assertion.arn, and add an attribute condition restricting assertion.account and assertion.arn.
D) Create a generic OIDC workload identity pool provider pointed at the AWS STS endpoint using default attribute mappings.

---

**Q9.** Engineers occasionally need roles/iam.securityAdmin to remediate incidents, but the security team wants this elevation to require manager approval, expire automatically after a set duration, and avoid any standing IAM role bindings. Which Google Cloud capability directly supports this workflow?

A) Configure an IAM Condition using the request.time attribute to restrict when the roles/iam.securityAdmin binding is active.
B) Create a Privileged Access Manager entitlement for roles/iam.securityAdmin that requires approval and defines a maximum activation duration.
C) Create an Access Context Manager access level that restricts elevated role usage to the corporate network.
D) Define a custom Organization Policy constraint that limits the duration of any IAM role binding.

---

**Q10.** A VPC Service Controls perimeter must only allow ingress from callers that are both connecting from an approved corporate IP range AND using a corporate-managed, encrypted device verified via Endpoint Verification — both conditions must be true simultaneously. How should this be configured in Access Context Manager?

A) Configure a VPC firewall rule that restricts ingress to the perimeter based on source IP tags only.
B) Create a single Access Context Manager access level with a basic condition combining IP subnetwork and device policy (corp-owned, encrypted) requirements using AND logic, then bind it to the perimeter.
C) Create two independent Access Context Manager access levels — one for IP range and one for device policy — and add both to the perimeter's access level list.
D) Create an Organization Policy constraint that restricts the resource location of perimeter-protected services.

---

**Q11.** A Cloud Identity administrator must ensure that every super admin account authenticates using a phishing-resistant second factor, explicitly disallowing SMS codes and printed backup codes as valid methods. Which configuration achieves this?

A) Enable 2-Step Verification enforcement on the Super Admins OU with the allowed method restricted to 'Security Key' only, disabling SMS and backup codes for that OU.
B) Enable 2-Step Verification for all users organization-wide with the allowed method set to 'Any'.
C) Configure Context-Aware Access to require connections to the Admin console originate from the corporate network.
D) Apply a password policy to the Super Admins OU requiring a minimum 20-character password.

---

**Q12.** A VPC Service Controls perimeter protects a BigQuery dataset in Project A. A Dataflow job running in Project B, which is outside the perimeter, needs to read from this dataset using one specific service account. The security team wants to allow only this exact service account to read only this specific dataset, without exposing any other resources or identities. Which configuration meets this requirement?

A) Create an ingress rule specifying the service account as the identity and the BigQuery dataset as the target resource
B) Add Project B to the perimeter as a dry-run project
C) Create an egress rule specifying the service account as the identity and BigQuery as the external service
D) Define an access level using the service account's identity and attach it to the perimeter's access policy

---

**Q13.** A company runs a SaaS analytics platform in its own VPC and needs to expose one specific internal service to hundreds of customer VPCs that belong to different organizations. The company cannot require customers to coordinate non-overlapping IP ranges, and does not want to expose its full VPC CIDR range to any consumer. Which connectivity option should it use?

A) VPC Network Peering
B) Shared VPC
C) Private Service Connect
D) Cloud VPN with static routes

---

**Q14.** An organization's Cloud NAT gateway uses manual port allocation with a static minimum of 64 ports per VM instance. During a load test, some VM instances exhaust their allocated ports and fail to open new outbound connections, while other instances on the same gateway have unused port capacity. What should the team do to resolve this while keeping per-instance allocation predictable?

A) Switch to Auto allocate ports for the entire NAT gateway
B) Enable Dynamic Port Allocation with a minimum and maximum ports per VM setting
C) Increase the number of external IP addresses assigned to the NAT gateway
D) Enable endpoint-independent mapping on the NAT gateway

---

**Q15.** A company requires a 99.99% availability SLA for an HA VPN connection between its on-premises data center and a VPC network. Which topology is required to achieve this SLA level?

A) Configure two on-premises VPN gateways/devices, each connected to both of the HA VPN gateway's two interfaces, forming four tunnels total
B) Configure two VPN tunnels from a single HA VPN gateway interface to two different peer VPN gateway interfaces
C) Use Classic VPN with two tunnels in an active-passive configuration
D) Configure a single tunnel with BGP route priority failover

---

**Q16.** A security team must allow outbound internet access from VM instances in a private subnet to only specific SaaS domains (for example, *.salesforce.com) over HTTPS, block all other outbound web destinations, and retain visibility into which service account made each web request. Which Google Cloud service should they deploy?

A) Cloud NAT with a custom route table for allowed destinations
B) Secure Web Proxy
C) Google Cloud Armor with URL Maps
D) Firewall rules using FQDN-based network objects

---

**Q17.** Which statement correctly describes Cloud DNS Response Policy Zones (RPZs) in the context of network boundary security design?

A) RPZs encrypt DNS queries between VMs and Cloud DNS resolvers using DNSSEC
B) RPZs enforce mutual TLS authentication for all DNS queries within a VPC
C) RPZs are used to define VPC Service Controls access levels based on DNS query patterns
D) RPZs allow overriding DNS responses for specific domains to block resolution of known malicious domains or redirect internal domains

---

**Q18.** A bank's on-premises data center connects to Google Cloud exclusively via a Dedicated Interconnect (no internet path). A VPC Service Controls perimeter protects a BigQuery project. On-premises analysts must query BigQuery over the Interconnect, and requests must be recognized as inside the perimeter so VPC-SC does not block them. Which configuration BEST satisfies this?

A) Enable Private Google Access on the VPC subnets used by the on-premises route advertisements
B) Configure Cloud NAT on the VPC so on-premises traffic appears to originate from a Google-owned external IP
C) Add the on-premises public IP range as an access level in an ingress policy attached to the perimeter
D) Create a Private Service Connect endpoint in the VPC for Google APIs (using the restricted.googleapis.com VIP) and route on-premises traffic to it over the Interconnect

---

**Q19.** A security team enables a VPC Service Controls perimeter around a project containing Cloud Storage and Artifact Registry. GKE nodes in the same VPC have no external IPs and rely on Private Google Access. After enabling the perimeter, image pulls from Artifact Registry start failing with a VPC-SC 'access denied' error, even though the GKE service account has the correct IAM roles. What is the MOST likely cause?

A) The nodes are resolving private.googleapis.com instead of restricted.googleapis.com, so traffic is not recognized as coming from an authorized VPC-SC network
B) The subnet's Private Google Access flag was disabled during the VPC-SC rollout
C) Cloud NAT is required for Artifact Registry pulls and was not configured
D) GKE workload identity federation is incompatible with VPC Service Controls

---

**Q20.** VM instances with no external IP addresses must be allowed to reach a small, approved allowlist of external SaaS domains over HTTPS (e.g., github.com, registry.npmjs.org) for CI build agents, with per-URL logging and TLS-aware policy enforcement, while all other outbound internet destinations must be blocked. The team wants a fully managed Google Cloud service rather than deploying and patching a third-party proxy VM. Which control BEST meets this requirement?

A) Cloud NAT combined with hierarchical firewall policy deny rules for 0.0.0.0/0
B) Secure Web Proxy with a URL list resource restricting egress to the approved domains
C) Cloud Armor security policy attached to a global external Application Load Balancer
D) Identity-Aware Proxy configured for TCP forwarding on the CI subnet

---

**Q21.** A security team wants to automatically block DNS resolution and connections to known command-and-control (C2) and malware-distribution domains for all workloads on a Shared VPC, using a continuously updated, Google-curated threat feed rather than a manually maintained domain blocklist. Which capability BEST satisfies this requirement?

A) A Cloud DNS Response Policy Zone populated by the security team with domains gathered from open-source threat feeds
B) Cloud Armor named IP lists applied to a global external Application Load Balancer
C) Cloud Next Generation Firewall Enterprise threat intelligence, using Google-curated dynamic address groups and FQDN objects in a firewall policy
D) A VPC Service Controls egress rule restricting access to an explicit list of external service perimeters

---

**Q22.** A financial services company must retain the ability to instantly and verifiably destroy all encryption key material for regulatory reasons, and the key material must be generated and stored entirely outside of Google's infrastructure at all times, with Google Cloud only calling out to request cryptographic operations. Which approach BEST satisfies this requirement?

A) Enable Google default encryption with Google-managed automatic key rotation
B) Configure Cloud External Key Manager (Cloud EKM) with keys hosted in a supported external key management partner system
C) Configure Customer-Managed Encryption Keys (CMEK) with keys generated directly inside Cloud KMS
D) Use the Cloud KMS Import Job feature to import externally generated key material into Cloud KMS

---

**Q23.** A company needs to create a test dataset from production credit card numbers for a legacy application whose validation logic depends on the token being the exact same length and character set (digits only) as the original card number. Which Sensitive Data Protection (DLP) transformation should be used?

A) Cryptographic hashing of each value
B) Date shifting
C) Format-preserving encryption (FPE) via the DLP API
D) Bucketing into value ranges

---

**Q24.** Your organization stores API keys as secrets in Secret Manager. A compliance auditor requires that every call to retrieve a secret's payload (accessSecretVersion) be recorded in Cloud Audit Logs. You confirm that Admin Activity logs are already capturing configuration changes to the secrets, but payload access calls are not appearing anywhere in Cloud Logging. What must you configure to close this gap?

A) Enable Data Access audit logs for the Secret Manager API and select the Data Read log type
B) Nothing — Admin Activity audit logs already capture secret payload access by default
C) Enable Data Access audit logs for the Secret Manager API and select the Admin Write log type
D) Deploy a Cloud Function triggered on every Secret Manager API call to manually write custom log entries

---

**Q25.** A bank and a credit bureau want to jointly compute a fraud risk score using each other's sensitive raw datasets. Neither organization is willing to expose its raw data to the other, and neither wants even the code author or Google to be able to view the data during processing. Which Google Cloud capability is designed for this use case?

A) A shared VPC Service Controls perimeter spanning both organizations' projects
B) Confidential Space using workload identity federation and remote attestation
C) Separate Cloud KMS CMEK keys issued to each organization for their respective datasets
D) Sensitive Data Protection (DLP) de-identification of both datasets before the joint computation

---

**Q26.** You rotate a Cloud KMS CMEK key used to protect a Cloud Storage bucket by creating a new primary key version. What happens to the objects that were already encrypted using the previous key version?

A) Cloud Storage automatically re-encrypts all existing objects with the new key version immediately after rotation
B) The old key version is automatically disabled at the moment of rotation, making the previously encrypted objects permanently unreadable
C) The previously encrypted objects remain decryptable using the old key version, which stays enabled until manually disabled or destroyed; existing ciphertext is not automatically re-encrypted
D) Rotation only affects newly created keys, not existing objects, meaning no change occurs to encryption at all until the bucket is deleted and recreated

---

**Q27.** A healthcare company must retain patient records in Cloud Storage for 7 years to satisfy regulatory requirements, and the retention guarantee must hold even against project owners or organization administrators attempting to delete or shorten it. Which configuration meets this requirement?

A) Enable Object Versioning and add a lifecycle rule to delete objects after 7 years
B) Restrict all IAM principals on the bucket to the Storage Object Viewer role only
C) Enable CMEK on the bucket and rely on key destruction timing to control data retention
D) Set a bucket-level retention policy of 7 years and lock it using Bucket Lock

---

**Q28.** A financial services company uses Cloud External Key Manager (Cloud EKM) to protect BigQuery datasets, with the actual key material hosted in an on-premises HSM through a certified EKM partner. The security team must immediately revoke all access to a dataset without destroying any data, and needs to know the real effect of their action. What should they do?

A) Delete the Cloud KMS key resource in Google Cloud; this automatically deletes the corresponding key in the external HSM, guaranteeing operations fail immediately
B) Rotate the Cloud KMS key to a new key version; Cloud EKM does not support multiple key versions, so data encrypted with the previous version becomes unreadable instantly
C) Revoke the IAM permission cloudkms.cryptoKeyVersions.useToEncrypt for all principals; this has no immediate effect because BigQuery caches decrypted data in its storage layer until the cache expires
D) Disable or revoke the key at the external key management partner; because Cloud KMS forwards every encrypt/decrypt request to the external system in real time with no local caching of key material, subsequent BigQuery operations requiring that key fail immediately

---

**Q29.** You need to de-identify 16-digit credit card numbers in a Cloud Storage-hosted dataset using Sensitive Data Protection (Cloud DLP). The output values must retain exactly the same length and character set (16 digits) as the originals so that downstream validation logic keeps working, and the transformation must be reversible only by principals who have access to the wrapped encryption key. Which crypto-based transformation should you configure?

A) CryptoHashConfig
B) DateShiftConfig
C) CryptoReplaceFfxFpeConfig
D) CryptoDeterministicConfig

---

**Q30.** Two independent healthcare organizations want to jointly train a diagnostic ML model using their combined patient datasets. Neither organization may see the other's raw data, and Google itself must not be able to access the data or the resulting model weights during processing. The parties also need cryptographic attestation that only the pre-agreed, unmodified workload code operated on the data. Which Google Cloud product is purpose-built for this scenario?

A) Confidential VM
B) Confidential Space
C) Cloud Key Management Service (KMS) with CMEK
D) VPC Service Controls

---

**Q31.** A company deploys retrieval-augmented generation agents on the Gemini Enterprise Agent Platform that query sensitive records in BigQuery and Cloud Storage. The security team must ensure that even if an agent's service account credentials are compromised, the attacker cannot copy or export this data into a Google Cloud project outside the organization's control. Which control should be implemented?

A) Configure a VPC Service Controls perimeter that encompasses the BigQuery, Cloud Storage, and Vertex AI services used by the agent platform, keeping all access confined within the perimeter boundary
B) Apply IAM Conditions on the agent's service account to restrict access to specific dataset and bucket resource names
C) Enable Confidential VM on the compute instances that host the agent runtime
D) Enable Cloud Audit Logs with Data Access log types for the BigQuery and Cloud Storage services

---

**Q32.** A platform team enabled Binary Authorization on a GKE cluster with a policy that requires an attestation from an attestor named 'vuln-scan-attestor' before any image can be deployed. The vulnerability scan step in Cloud Build passes, but the deployment step still fails because no valid attestation exists. Which action is required to allow the pipeline to produce a valid attestation that Binary Authorization will accept?

A) Grant the Cloud Build service account the roles/binaryauthorization.attestorsViewer role only
B) Add an organization policy constraint that blocks unattested images at the VPC Service Controls perimeter
C) Create a Container Analysis Note, associate it with the 'vuln-scan-attestor' Attestor resource, and sign the image digest using the KMS key linked to that attestor
D) Enable VPC Flow Logs on the GKE cluster's subnet so Binary Authorization can validate network attestation

---

**Q33.** A security team wants a single organization-level log sink that exports Cloud Audit Logs (Admin Activity and Data Access) from every project in the organization into a BigQuery dataset in a centralized security project, except logs originating from the 'sandbox-testing' folder, which should never be exported. Which configuration meets this requirement with the least administrative overhead?

A) Create an aggregated sink at the organization resource with includeChildren=true and an exclusion filter that matches logs originating from the sandbox-testing folder
B) Create a sink in each individual project except those under the sandbox-testing folder, all pointing to the centralized BigQuery dataset
C) Create a folder-level sink on every folder except sandbox-testing, with includeChildren set to false
D) Create an aggregated sink at the organization resource with includeChildren=true, and rely on default IAM permissions to prevent BigQuery access from the sandbox folder

---

**Q34.** Security engineers must detect known malware command-and-control signatures and other network-based threats within east-west traffic between Compute Engine VMs in a single VPC, without inserting an inline device that could add latency or become a single point of failure. Which Google Cloud service should they deploy?

A) VPC Flow Logs configured with a 100% sampling rate
B) Cloud NGFW Standard tier with hierarchical firewall policies
C) Secure Web Proxy deployed as a forward proxy for all egress traffic
D) Cloud IDS, which uses Packet Mirroring to send a copy of traffic to a managed, signature-based threat detection engine

---

**Q35.** An organization needs automated detection for two distinct requirements: (1) flag any Compute Engine instance whose resource configuration does not include a mandatory 'data-classification' label, and (2) flag log-based evidence of anomalous IAM permission grants performed via unusual API call sequences. Which pairing correctly matches each requirement to the Security Command Center capability designed for it?

A) Requirement 1 uses a Security Health Analytics custom module (CEL against resource configuration data); requirement 2 uses an Event Threat Detection custom module (YARA-L against log data)
B) Both requirements should use Event Threat Detection custom modules written in YARA-L
C) Both requirements should use Security Health Analytics custom modules written in CEL
D) Requirement 1 uses Container Threat Detection; requirement 2 uses VPC Service Controls custom access levels

---

**Q36.** A compliance mandate requires an immutable audit trail capturing every successful 'AccessSecretVersion' call made against secrets in Secret Manager, including calls made by project owners. The team left audit logging at its default configuration. A review shows no log entries exist for successful secret access calls. What must be changed?

A) Nothing — Cloud Audit Logs does not support Secret Manager; Cloud Trace must be used instead
B) Increase the Cloud Logging retention period for the _Required log bucket to 400 days
C) Enable Data Access audit logs (DATA_READ) for the Secret Manager API via an IAM Audit Config, since Data Access logs are disabled by default except for BigQuery
D) Enable VPC Flow Logs on the subnet hosting the Secret Manager service endpoint

---

**Q37.** A security team must ensure that all Compute Engine VM instances across an organization automatically receive critical OS security patches within 48 hours of release, and must be able to generate compliance reports showing patch status per instance. Which Google Cloud capability should they configure?

A) Configure VM Manager patch policies (OS Patch Management) with a defined schedule and recurrence, and use Patch Compliance reporting for auditing
B) Configure Security Health Analytics to flag unpatched instances after scanning instance metadata weekly
C) Enable Container Analysis vulnerability scanning on the instances' base images in Artifact Registry
D) Create a VPC Service Controls perimeter restricting access to the Compute Engine API

---

**Q38.** An organization requires that only container images which have passed vulnerability scanning in CI, with zero CRITICAL or HIGH severity findings, can be deployed to any GKE cluster. Which combination of controls BEST enforces this requirement?

A) Configure Artifact Registry vulnerability scanning in the CI pipeline, and require a Binary Authorization attestation — created only when scan results meet the severity threshold — from a custom attestor before deployment is allowed
B) Enable Container Threat Detection so that any container found running with a known vulnerability is automatically terminated after deployment
C) Configure Binary Authorization with the built-in policy that requires attestations only from Google-built base images
D) Create a Security Health Analytics custom module that flags images with critical vulnerabilities, and have Cloud Build poll findings before triggering the deploy step

---

**Q39.** A security team wants to detect network-based threats such as port scanning, malware command-and-control callbacks, and protocol anomalies occurring in east-west traffic between VM instances in the same VPC, without deploying or managing a custom inspection appliance. Which Google Cloud capability should they enable?

A) VPC Flow Logs with sampling enabled at 100%
B) Firewall Rule Logging on all applicable firewall rules
C) Packet Mirroring to a self-managed third-party IDS appliance in the same VPC
D) Cloud IDS

---

**Q40.** A security engineer configures an organization-level aggregated log sink to export all audit logs from every project into a centralized BigQuery dataset. After deployment, Admin Activity audit logs appear correctly, but Data Access audit logs for Cloud Storage object read operations are completely missing from the dataset. What is the most likely cause?

A) The aggregated sink's inclusion filter must explicitly contain the clause logName:"data_access" or these logs are dropped by default
B) Aggregated sinks at the organization level can only capture Admin Activity logs; Data Access logs require a separate per-project sink configuration
C) Data Access audit logs for services other than BigQuery are disabled by default and must be explicitly enabled per service via an IAM Audit Config policy
D) The sink is exporting to the wrong destination type — Data Access logs can only be routed to a Cloud Storage bucket, not BigQuery

---

**Q41.** A US federal agency workload must meet FedRAMP High requirements, including restricting Google support engineer access to US persons only and keeping all data and metadata within US regions. The customer already uses standard IAM and Cloud KMS. Which additional control should be configured to meet BOTH the personnel and data residency requirements in a single, auditable configuration?

A) Create an Organization Policy constraint restricting resourceLocations to US regions, and rely on IAM conditions to block non-US support staff
B) Provision the workload inside an Assured Workloads folder using the FedRAMP High control package
C) Enable VPC Service Controls with a perimeter around all projects containing the workload
D) Configure CMEK with Cloud KMS keys created in a US multi-region keyring

---

**Q42.** An auditor requires that before any Google support or engineering employee can view customer content during an active support case, the customer must explicitly grant permission for that specific access, and denial of the request must be technically enforced (not just logged). Which Google Cloud capability satisfies this requirement?

A) Access Transparency logs
B) Cloud Audit Logs Data Access logs
C) Access Approval
D) Assured Workloads support restriction package

---

**Q43.** A security team must prevent any team in the organization from creating storage buckets, BigQuery datasets, or Compute Engine instances outside EU regions, and this restriction must apply automatically to newly created projects without manual per-project setup. No requirement exists for restricting Google support personnel. Which control best fits, with least operational overhead?

A) Set the constraints/gcp.resourceLocations organization policy at the organization node so it inherits to all folders and projects
B) Provision every new project inside an Assured Workloads folder using the EU regions control package
C) Configure a VPC Service Controls perimeter scoped to EU regions around all projects
D) Require each project owner to manually set a location parameter on every resource they create

---

**Q44.** An auditor reviewing the shared responsibility model asks who is responsible for key rotation policy and IAM permissions on a Cloud KMS key used for CMEK-encrypted Cloud Storage buckets, versus who is responsible for the underlying storage-layer encryption infrastructure. Which statement is accurate?

A) Google automatically rotates and manages all customer KMS keys on the customer's behalf once CMEK is enabled
B) The customer must implement application-layer encryption because CMEK does not encrypt data at rest for Cloud Storage
C) Enabling CMEK automatically extends Access Approval-equivalent protections, removing the need for separate Access Approval configuration
D) The customer is responsible for managing key rotation schedules and IAM bindings on the KMS key; Google is responsible for the underlying storage encryption infrastructure and hardware security

---

**Q45.** A regulation requires the organization to produce forensic evidence of every instance where a user or service account read data from a specific set of sensitive BigQuery tables, retained for 7 years in a tamper-resistant location. Which configuration satisfies this requirement?

A) Rely on the default Admin Activity audit logs, which are enabled and retained automatically at no cost
B) Enable Data Access audit logs for BigQuery Data Read events, and export them to a locked Cloud Storage bucket with a 7-year retention policy
C) Enable VPC Flow Logs on all subnets used by BigQuery client applications and retain them for 7 years
D) Enable Access Transparency logging on the organization to capture all read activity against the sensitive tables

---

