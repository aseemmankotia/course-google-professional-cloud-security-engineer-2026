## Chapter 11 Cheat Sheet — Shared Responsibility, Assured Workloads, Compliance

### Shared Responsibility Boundary Shift
| Service Type | Example | Customer Owns | Google Owns |
|---|---|---|---|
| IaaS | Compute Engine | Guest OS, patching, app, data, IAM/firewall config | Hypervisor, physical infra, host OS |
| Managed Container | GKE Standard | Node OS/patching, workload config | Control plane |
| Managed Container (serverless) | GKE Autopilot | Workload/pod config only | Node provisioning, patching, node security |
| Fully Managed / SaaS-like | BigQuery, Cloud SQL (partial) | Data classification, IAM, data-level access | OS, runtime, platform patching, infra |

Rule of thumb: the more 'managed' the service, the more the boundary shifts toward Google — customer responsibility narrows toward **data + access control**, never disappears entirely.

### Assured Workloads — Key Control Packages
| Package | Use Case |
|---|---|
| FedRAMP Moderate/High | US civilian federal agencies |
| IL4 / IL5 | DoD workloads (IL5 = higher sensitivity) |
| CJIS | Criminal justice information |
| HIPAA | US healthcare data |
| IRS 1075 | Federal Tax Information |
| ITAR | US export-controlled data, US-persons-only personnel |
| EU Regions and Support | EU data residency + EU support personnel |
| Canada Regions and Support (Protected B) | Canadian residency + Protected B |

Assured Workloads enforces: data residency, personnel access restrictions, and (for some packages) restricted service usage — all inherited automatically by resources in the folder.

### Access Transparency vs Access Approval
| Feature | Purpose | Timing | Blocking? |
|---|---|---|---|
| Access Transparency | Log of Google personnel access + justification | Near real-time, after access | No — visibility only |
| Access Approval | Requires explicit customer approval | Before access occurs | Yes — gates access (emergency override configurable) |

Memory trick: **Trans**parency = **see** it. **Approval** = **stop** it until you say yes.

### Cloud Audit Logs Retention
| Log Type | Default State | Can Disable? | Retention |
|---|---|---|---|
| Admin Activity | On | No | 400 days |
| System Event | On | No | 400 days |
| Data Access (non-BQ) | Off | N/A (enable to use) | 30 days default |
| Data Access (BigQuery) | On | Yes | 30 days default |
| Policy Denied | On | Yes | 30 days |

### Org Policy for Data Residency
- Constraint: `constraints/gcp.resourceLocations`
- Values: specific regions, or groups like `in:eu-locations`, `in:us-locations`
- Applies at org/folder/project level; inherited down the hierarchy
- Common exam trap: confusing this with VPC Service Controls (which restricts data *movement/exfiltration*, not resource *location*)

### Mapping Compliance to Controls
| Requirement | Control |
|---|---|
| Data must stay in region X | Org policy: resourceLocations |
| No unapproved Google access to data | Access Approval |
| Need audit trail of Google access | Access Transparency |
| Restricted support personnel geography/clearance | Assured Workloads control package |
| Prevent data exfiltration across projects | VPC Service Controls (separate domain, but often paired) |
