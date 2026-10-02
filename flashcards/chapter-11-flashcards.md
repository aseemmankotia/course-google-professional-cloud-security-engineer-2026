## Chapter 11: Shared Responsibility, Assured Workloads, and Compliance Scoping — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Admin Activity audit log retention period, and can it be disabled? | 400 days retention; cannot be disabled. Exam tip: always-on regardless of service. |
| 2 | Data Access audit log default retention and default enablement state? | 30 days default retention; disabled by default for most services (BigQuery Data Access logs are an exception — on by default). Exam tip: must explicitly enable for most services, which may incur cost. |
| 3 | System Event and Policy Denied audit logs: on by default? Can disable? Retention? | System Event: always on, cannot disable, 400 days. Policy Denied: on by default, CAN be disabled, 30 days. |
| 4 | Shared responsibility: Compute Engine vs BigQuery — who patches the guest OS? | Compute Engine: customer patches guest OS. BigQuery (fully managed): Google owns OS/runtime entirely; customer only manages data and access controls. |
| 5 | GKE (standard) vs GKE Autopilot: node management responsibility? | Standard GKE: customer manages node pools, OS patching, node config. Autopilot: Google manages nodes entirely — customer focuses on workloads/pods only. |
| 6 | Access Transparency vs Access Approval — one-line distinction | Access Transparency = visibility (near real-time logs of Google access, after the fact). Access Approval = control (requires explicit customer approval BEFORE access, with emergency override). |
| 7 | Which Assured Workloads control package targets DoD Impact Level 5 workloads? | IL5. Exam tip: don't confuse with IL4 (lower sensitivity) or FedRAMP High (civilian, not DoD). |
| 8 | Which Assured Workloads control package enforces Canadian data residency + Protected B controls? | Canada Regions and Support (Protected B). Restricts data location and support personnel to Canada. |
| 9 | Organization policy constraint to restrict resource creation location (data residency)? | constraints/gcp.resourceLocations — supports value groups like in:eu-locations, in:us-locations, or specific region names. |
| 10 | gcloud command pattern to view current org policy on resource locations | gcloud resource-manager org-policies describe constraints/gcp.resourceLocations --organization=ORG_ID |
| 11 | Which Assured Workloads package maps to healthcare data protection requirements (US)? | HIPAA control package. Exam tip: distinct from IRS 1075 (federal tax info) and CJIS (criminal justice data). |
| 12 | Access Approval emergency override — what does it do? | Allows Google to access data in true emergencies without prior approval; admins can disable this override entirely for stricter fail-closed enforcement. Exam tip: tested as the exception to the 'no access without approval' rule. |
