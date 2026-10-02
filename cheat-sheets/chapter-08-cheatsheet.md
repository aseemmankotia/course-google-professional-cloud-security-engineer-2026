# Ch.8 Cheat Sheet — Storage Lifecycle & AI/ML Data Protection

## Lifecycle Rule Conditions & Actions
| Condition | Meaning |
|---|---|
| `age` | days since object creation |
| `createdBefore` | RFC3339 date |
| `isLive` | true = current version, false = noncurrent |
| `numNewerVersions` | # newer versions exist |
| `daysSinceCustomTime` / `customTimeBefore` | requires `customTime` metadata set, else rule never fires |
| `daysSinceNoncurrentTime` / `noncurrentTimeBefore` | versioned objects only |

**Actions:** `SetStorageClass` (target class) or `Delete`. **Max 100 rules/bucket.** Rules evaluated ~once/day, not instantly.

## Storage Class Minimum Duration & Early Deletion
| Class | Min Duration | Early Deletion Fee If Deleted/Reclassed Sooner |
|---|---|---|
| Standard | none | no |
| Nearline | 30 days | yes |
| Coldline | 90 days | yes |
| Archive | 365 days | yes |

## Retention Policy & Bucket Lock (WORM)
- `gsutil retention set <period> gs://BUCKET` — sets min retention for ALL objects in bucket.
- `gsutil retention lock gs://BUCKET` — **irreversible**; after locking, retention can only be INCREASED, never reduced/removed. Bucket cannot be deleted until all objects clear retention.
- Max retention period: **100 years**.

## Object Holds
| Hold Type | Behavior | Use Case |
|---|---|---|
| Temporary hold | Blocks delete/overwrite until manually released; no effect on retention clock | Litigation / legal hold |
| Event-based hold | Blocks delete/overwrite; releasing it **starts** the retention-period countdown | Retention that begins at a triggering event (e.g., doc finalized) |

## IAM vs VPC Service Controls
| Control | Question Answered | Stops Exfiltration w/ Valid Creds? |
|---|---|---|
| IAM | Who can call this API? | No |
| VPC-SC | From what context/perimeter can data move? | Yes |

**Pattern:** Wrap Cloud Storage + BigQuery + Vertex AI APIs in ONE perimeter to protect training data & model artifacts end-to-end.

## Protecting Training Data
- **Sensitive Data Protection (DLP):** inspect + de-identify (mask/tokenize/redact) PII before it reaches a training bucket.
- Pattern: raw → staging bucket → DLP job → de-identified bucket (inside VPC-SC perimeter) → training job.

## Model Artifacts & Endpoints
- Scope IAM narrowly (custom roles) instead of broad `aiplatform.admin`.
- Use **private endpoints** (VPC Network Peering or Private Service Connect) for online prediction — no public IP.
- Model Registry access controlled via IAM at project/resource level.

## Gemini Enterprise Agent Platform
- Access control = IAM on the **grounding data store/connector** + agent's **runtime service account** roles (least privilege).
- Agent config alone ≠ data access control — the underlying data source ACL is authoritative.

## Data Residency
- Org Policy constraint: **`gcp.resourceLocations`** restricts where resources (buckets, Vertex AI resources) can be created.
- Combine with regional Cloud Storage buckets + regional Vertex AI endpoints for enforced (not just documented) residency.
