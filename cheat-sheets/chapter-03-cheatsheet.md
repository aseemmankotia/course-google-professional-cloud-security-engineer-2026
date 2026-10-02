## Chapter 3 Cheatsheet: IAM Authorization, Deny Policies, Org Policy

### Role Types
| Type | Example | Notes |
|---|---|---|
| Basic | Owner, Editor, Viewer | Broad, legacy, avoid on exam 'least privilege' scenarios |
| Predefined | roles/compute.admin | Google-managed, service-scoped, granular |
| Custom | myOrg.customRole | Project or org only — NOT folder level. Max 300/project or org, 3,000 permissions/role |

### Allow vs Deny vs Org Policy
| Mechanism | Governs | Overridable lower in hierarchy? | Precedence |
|---|---|---|---|
| IAM Allow (role binding) | Who can do what | Adds, cannot be reduced below | Union across hierarchy |
| IAM Condition | Attribute-based allow (time, resource tag/name) | Attached to one binding | Evaluated as part of allow |
| IAM Deny Policy | Who is blocked from what, org-wide | No — deny always wins | Evaluated first, overrides all allow |
| Org Policy Constraint | Resource configuration allowed | Yes, by default (closest node wins) | Independent of IAM entirely |

### Org Policy Constraint Types
| Type | Values | Example |
|---|---|---|
| Boolean | enforced true/false | constraints/iam.disableServiceAccountKeyCreation |
| List | allowed_values / denied_values | constraints/gcp.resourceLocations |

### Resource Hierarchy
`Organization > Folder > Project > Resource` — allow policies inherit downward additively. Deny policies also apply hierarchically but cannot be countermanded by a lower-level allow. Org policies use closest-node-wins unless authored with hierarchical/conditional rules.

### Access Context Manager
- Access Levels evaluate: IP range, device policy, geography (NOT time).
- Consumed by: Identity-Aware Proxy (app-level context-aware access) and VPC Service Controls (perimeter-level).

### Privileged Access Manager (PAM)
- Just-in-time, time-bound entitlement grants.
- Max grant duration: up to 14 days.
- Requires approval workflow; auto-revokes; fully audit-logged.

### Policy Intelligence Tools
| Tool | Purpose |
|---|---|
| Policy Analyzer | Who has access to what, right now (Cloud Asset Inventory) |
| Policy Simulator | Test a proposed IAM change before applying |
| IAM Recommender | Suggests trimming unused/over-broad grants |

### Common Exam Traps
- Picking IAM Conditions when the scenario demands an unbreakable guardrail (needs Deny Policy).
- Assuming Org Policy can't be overridden lower in hierarchy — by default it CAN, unlike Deny Policy.
- Confusing Access Context Manager (device/location) with IAM Conditions (time/resource tag).
- Forgetting custom roles cannot be created at the folder level.
