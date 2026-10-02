## Chapter 4: Perimeter Controls: Cloud NGFW, IAP, and Cloud Armor — Practice Questions

**Q1.** Employees at a company need to access an internal HR web app from unmanaged home laptops. Security requires that access is granted only if the device is verified and the user passes MFA, without deploying a VPN client. Which solution BEST meets these requirements?

A) Cloud VPN with a shared PSK and OS Login
B) Identity-Aware Proxy with a context-aware access level requiring device verification and MFA
C) Cloud Armor with a reCAPTCHA-based security policy
D) Cloud NAT combined with a restrictive VPC firewall rule

<details>
<summary>Answer</summary>

**Correct: B**

IAP terminates access at the HTTPS load balancer and enforces context-aware access levels (device state, MFA) per request, replacing the need for a VPN.

**Why the others are wrong:**
- Cloud VPN still requires a client and does not evaluate device posture per request
- Cloud Armor protects against web attacks/DDoS, it does not perform identity or device-based access control
- Cloud NAT is for outbound internet access, not inbound identity-based access control

</details>

---

**Q2.** An organization's security team wants an SSH-deny rule for a sensitive IP range that folder and project administrators cannot override, regardless of what VPC firewall rules they create. How should this be implemented?

A) A hierarchical firewall policy attached at the organization node with an explicit deny action and no goto_next continuation
B) A VPC firewall rule at priority 0 in every project
C) A hierarchical firewall policy attached at the folder node with action set to goto_next
D) A Cloud Armor security policy scoped to the organization

<details>
<summary>Answer</summary>

**Correct: A**

Hierarchical firewall policies are evaluated top-down starting at the organization node before folder policies, network firewall policies, and legacy VPC rules. An explicit deny rule without goto_next stops evaluation, so lower levels cannot override it.

**Why the others are wrong:**
- VPC firewall rules are evaluated last and can be added/removed by project owners, so they are overridable
- A rule attached at the folder node can still be overridden by a higher-priority organization-level rule and doesn't guarantee org-wide enforcement
- goto_next explicitly allows evaluation to continue to lower levels, which is the opposite of non-overridable

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** Your public-facing web application behind a global external Application Load Balancer is receiving SQL injection attempts. You need to block these requests immediately without changing application code. Which service and configuration addresses this fastest?

A) Enable Cloud NGFW Enterprise IPS on the VPC network
B) Add an IAP context-aware access level requiring a corporate device
C) Configure a hierarchical firewall policy denying the attacker's source IP
D) Attach a Cloud Armor security policy with the preconfigured SQLi WAF rule to the backend service

<details>
<summary>Answer</summary>

**Correct: D**

Cloud Armor's preconfigured WAF rules (based on OWASP ModSecurity CRS) can be attached to a backend service in minutes and inspect Layer 7 payloads for SQLi patterns, blocking malicious requests at the edge.

**Why the others are wrong:**
- Cloud NGFW Enterprise IPS inspects network traffic within the VPC/hybrid environment, not HTTP payloads terminating at the global load balancer
- IAP controls identity-based access, it does not inspect for injection attack patterns
- Blocking a single source IP does not stop distributed SQLi attempts from other IPs

</details>

---

**Q4.** A company needs to issue short-lived TLS certificates for internal microservices, integrated into an existing enterprise PKI hierarchy, without paying a public CA. Which service should they use?

A) Certificate Authority Service (CAS) configured as a subordinate CA
B) Google-managed SSL certificates on the load balancer
C) Cloud KMS symmetric encryption keys
D) Secret Manager with automatic rotation

<details>
<summary>Answer</summary>

**Correct: A**

CAS lets you create a private root or subordinate CA that chains to an existing enterprise root, issuing short-lived internal-use certificates on demand via API.

**Why the others are wrong:**
- Google-managed SSL certificates are for public domains fronting Google load balancers and cannot chain to a private enterprise root
- Cloud KMS manages encryption keys, not X.509 certificate issuance
- Secret Manager stores secrets/certificates but does not issue or sign certificates

</details>

---

**Q5.** A fleet of Compute Engine VMs must only reach a specific allowlist of external package-repository domains for outbound HTTP/HTTPS traffic, without operators managing proxy VMs.

A) Cloud NAT with a restrictive route table
B) Cloud Armor edge security policy
C) Secure Web Proxy (SWP) with an allowed FQDN policy
D) Cloud DNS private zone with forwarding

<details>
<summary>Answer</summary>

**Correct: C**

Secure Web Proxy is a fully managed forward proxy that filters and allowlists outbound web traffic by FQDN/URL without requiring self-managed proxy instances.

**Why the others are wrong:**
- Cloud NAT provides address translation for outbound traffic, not application-layer domain filtering
- Cloud Armor protects inbound traffic to load balancers, not VM egress
- Cloud DNS private zones control name resolution, not traffic filtering

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** You need Cloud Armor to allow up to 100 requests per minute per client IP on an API endpoint, and any client exceeding that limit should be blocked for the next 10 minutes. Which rate limiting configuration is correct?

A) action: rate_based_ban, rate_limit_threshold count=100/interval=60s, ban_duration_sec=600
B) action: throttle, rate_limit_threshold count=100/interval=60s, no ban duration
C) action: deny(403) applied to all traffic on the backend service
D) action: redirect to a CAPTCHA page for every request

<details>
<summary>Answer</summary>

**Correct: A**

rate_based_ban blocks a client for a configurable ban_duration_sec once it exceeds the configured threshold, which matches the '10 minute block' requirement.

**Why the others are wrong:**
- throttle only enforces the rate limit per interval; it does not impose an extended ban afterward
- A blanket deny would block all legitimate traffic, not just clients exceeding the threshold
- Redirecting every request to CAPTCHA is not a rate limiting action and impacts all users, not just abusive ones

</details>

---

**Q7.** What specific threat does enabling DNSSEC on a Cloud DNS managed zone protect against?

A) Volumetric DDoS floods against the DNS resolver
B) Encrypting the contents of DNS queries in transit
C) Rate limiting excessive DNS query volume per client
D) DNS response spoofing and cache poisoning, by cryptographically validating that responses came from the authoritative zone

<details>
<summary>Answer</summary>

**Correct: D**

DNSSEC signs zone records (RRSIG/DNSKEY) so resolvers can verify that a response genuinely originated from the authoritative zone and was not tampered with, preventing spoofing/cache poisoning.

**Why the others are wrong:**
- DNSSEC does not mitigate volumetric DDoS; that is handled by Cloud Armor/network-level protections
- DNSSEC authenticates responses but does not encrypt query content — that's the role of DNS-over-HTTPS/TLS
- DNSSEC has no rate limiting function

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** An organization-level hierarchical firewall policy rule allows SSH from range 203.0.113.0/24 at priority 100 with no goto_next. A project owner later adds a VPC firewall rule at priority 1000 that denies all SSH traffic. From a client inside 203.0.113.0/24, what happens?

A) SSH is allowed, because the hierarchical firewall policy is evaluated before VPC firewall rules and the explicit allow without goto_next stops further evaluation
B) SSH is denied, because the VPC rule has a numerically lower priority value than 1000 makes no difference — VPC rules always win
C) SSH is denied, because project-level rules always override organization-level policy
D) The outcome depends entirely on whether IAP TCP forwarding is configured

<details>
<summary>Answer</summary>

**Correct: A**

Hierarchical firewall policies (org, then folder) are evaluated before VPC network firewall rules. An explicit allow rule without goto_next terminates evaluation for matching traffic, so the lower-tier VPC deny rule is never reached.

**Why the others are wrong:**
- Priority values are compared only within the same policy tier, not across tiers; org-level policy is enforced first regardless of the VPC rule's priority number
- Project-level VPC rules do not override org-level hierarchical policy when the hierarchical rule terminates evaluation
- IAP TCP forwarding is unrelated unless the firewall explicitly allows the 35.235.240.0/20 IAP range; it does not change this evaluation order

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

