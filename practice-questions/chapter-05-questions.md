## Chapter 5: VPC Service Controls, Shared VPC, and Private Connectivity — Practice Questions

**Q1.** A company stores sensitive data in BigQuery and Cloud Storage. Security team is concerned that a compromised service account with valid IAM permissions could exfiltrate data by copying it to a Cloud Storage bucket in an attacker-controlled project. Which control BEST addresses this risk?

A) Restrict egress with VPC firewall rules on all subnets
B) Add IAM Conditions requiring MFA on the service account
C) Create a VPC Service Controls perimeter around the projects containing the sensitive resources
D) Deploy Cloud Armor in front of the BigQuery API

<details>
<summary>Answer</summary>

**Correct: C**

VPC Service Controls create a service-level perimeter around managed services (BigQuery, GCS) that blocks data movement to resources outside the perimeter, even if IAM permissions would otherwise allow the copy.

**Why the others are wrong:**
- Firewall rules control network/IP traffic, not API calls to Google-managed services like BigQuery/GCS, which don't route through customer VPC firewalls
- IAM Conditions restrict when/how access is granted but don't stop an authorized principal from copying data outside the org boundary
- Cloud Armor protects HTTP(S) load balancers from web attacks; it has no role in API-level data exfiltration prevention

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** An on-premises network needs to privately consume a SaaS product that the vendor has published as a service attachment, without establishing VPC Peering or exposing the vendor's internal network. Which service should be used?

A) VPC Peering
B) Cloud NAT
C) Private Google Access
D) Private Service Connect

<details>
<summary>Answer</summary>

**Correct: D**

Private Service Connect lets a consumer create an internal IP endpoint in their VPC that privately connects to a producer's published service attachment, with no peering and no network exposure in either direction.

**Why the others are wrong:**
- VPC Peering requires both sides to expose full network routes and cannot be used to consume a published service attachment this way
- Cloud NAT only provides outbound internet access for instances without external IPs; it doesn't connect to another organization's services
- Private Google Access only allows reaching Google APIs, not third-party SaaS service attachments

</details>

---

**Q3.** A central networking team must retain full control over firewall rules, subnets, and routes across two application teams' VMs, while each team can independently deploy and manage their own compute resources. Which network design meets this requirement?

A) VPC Peering between two separate VPCs, one per team
B) Two standalone VPCs connected via Cloud VPN tunnels
C) A single VPC with per-subnet IAM roles only, no host/service project split
D) Shared VPC with the networking team owning the host project and teams as service projects

<details>
<summary>Answer</summary>

**Correct: D**

Shared VPC's host/service project model centralizes network administration (firewalls, subnets, routes) in the host project while service project admins retain roles/compute.networkUser to deploy resources into shared subnets without network control.

**Why the others are wrong:**
- VPC Peering gives each network independent control over its own firewall rules and routes — the opposite of centralized control
- Separate VPCs over VPN still means each team manages their own network, not centralized administration
- Without host/service project separation there's no mechanism to separate network admin duties from resource deployment duties

</details>

---

**Q4.** You need Compute Engine VMs that have no external IP addresses to reach Cloud Storage and other Google APIs. What must you configure?

A) Assign each VM an external IP with a restrictive firewall rule
B) Enable Private Google Access on the subnet
C) Configure Cloud NAT for the VPC
D) Establish VPC Peering with Google's network

<details>
<summary>Answer</summary>

**Correct: B**

Private Google Access is a per-subnet setting that lets instances without external IPs reach Google APIs and services over Google's internal network paths.

**Why the others are wrong:**
- Assigning external IPs is unnecessary and against the requirement of having no external IP; it also increases attack surface
- Cloud NAT provides general outbound internet access but Private Google Access is the purpose-built, more restrictive mechanism specifically for Google APIs
- VPC Peering with Google's network is not a real construct — Google APIs are reached via Private Google Access, not peering

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** After enabling a VPC Service Controls perimeter around a project, a legitimate nightly job that exports BigQuery data to a partner-owned Cloud Storage bucket outside the perimeter starts failing. Without disabling perimeter protection, what should you do?

A) Move the partner's bucket inside the perimeter
B) Disable VPC Service Controls for the BigQuery API only
C) Grant the service account the Owner role on the partner's project
D) Add an egress rule to the perimeter policy allowing the specific service account to access the external resource

<details>
<summary>Answer</summary>

**Correct: D**

Egress rules are the designed mechanism to allow specific identities/resources inside a perimeter to reach specific resources outside it, without weakening overall perimeter enforcement.

**Why the others are wrong:**
- You don't control or own the partner's project/bucket, so you cannot move it into your perimeter
- Disabling VPC-SC for an API reopens the exact exfiltration risk the perimeter was built to prevent
- Granting Owner role addresses IAM permissions, not the VPC-SC perimeter block, and is a major over-permission risk

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** Which statement about VPC Peering is correct?

A) Peering is transitive — if A peers with B and B peers with C, A can reach C
B) Peered networks may have overlapping CIDR ranges
C) Peering requires a Cloud VPN tunnel to be established first
D) Each peered network independently manages its own firewall rules and routes

<details>
<summary>Answer</summary>

**Correct: D**

VPC Peering connects two networks at the routing layer, but each side retains full independent control of its own firewall rules, routes, and IAM.

**Why the others are wrong:**
- Peering is explicitly non-transitive — A cannot reach C through B without a direct A-C peering connection
- Overlapping CIDR ranges are not allowed and will prevent the peering connection from being established
- Peering uses Google's internal network backbone, not VPN tunnels; no VPN is required

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** A company needs a private connection between their on-premises data center and GCP with 10 Gbps of dedicated capacity that does not traverse the public internet. Which service should they use?

A) Cloud NAT
B) Dedicated Interconnect
C) HA VPN
D) Private Service Connect

<details>
<summary>Answer</summary>

**Correct: B**

Dedicated Interconnect provides a direct, private physical connection between on-premises and Google's network at 10 Gbps or 100 Gbps per circuit, bypassing the public internet.

**Why the others are wrong:**
- Cloud NAT is for outbound internet access from GCP resources, unrelated to on-prem hybrid connectivity
- HA VPN encrypts traffic over the public internet; it does not provide a dedicated private physical link
- Private Service Connect is for consuming published services privately within/between VPCs, not for general on-prem-to-GCP bulk connectivity

</details>

---

**Q8.** A GKE cluster uses private nodes with no external IP addresses. The nodes need to pull container images from a public internet-hosted registry. What should you configure?

A) Cloud NAT for the VPC
B) Private Google Access on the subnet
C) VPC Peering to the registry's network
D) Private Service Connect endpoint

<details>
<summary>Answer</summary>

**Correct: A**

Cloud NAT provides outbound-only internet access for instances without external IPs, which is required to reach a public (non-Google) internet-hosted registry.

**Why the others are wrong:**
- Private Google Access only covers Google APIs and services (e.g., googleapis.com), not arbitrary public internet registries
- You cannot peer with an arbitrary public internet registry's network — peering is a GCP-to-GCP VPC construct
- Private Service Connect requires the registry to publish a service attachment; a generic public registry does not offer this

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

