## Chapter 6 Cheat Sheet — Data Protection

### Sensitive Data Protection (DLP) De-identification Techniques
| Technique | Reversible? | Format Preserved? | Use Case |
|---|---|---|---|
| RedactConfig | No | No (removed) | Delete sensitive value entirely |
| CharacterMaskConfig | No | Partial (masked chars) | Show last N digits only |
| CryptoHashConfig | No (one-way) | No | Irreversible anonymization, consistent output |
| CryptoDeterministicConfig | Yes (with key + surrogate) | No | Reversible pseudonymization, consistent tokens |
| CryptoReplaceFfxFpeConfig | Yes (with key) | Yes (FPE) | Tokenize while keeping original format (e.g., card numbers) |
| DateShiftConfig | Context-dependent | Yes (still a date) | Shift dates per-entity while preserving intervals |
| BucketingConfig | No | No (range only) | Generalize numeric/date values into buckets |

### BigQuery Access Control Layers
| Mechanism | Grain | Notes |
|---|---|---|
| IAM roles (dataViewer, etc.) | Dataset/table/project | Broadest control |
| Authorized View | View → source table | View lives in separate dataset; grantee never touches source table |
| Row Access Policy | Row | `CREATE ROW ACCESS POLICY ... FILTER USING (...)` |
| Column Policy Tags | Column | Requires Data Catalog/Dataplex taxonomy + Fine-Grained Reader role |

### Cloud Storage: IAM vs ACL
| Feature | IAM | ACL |
|---|---|---|
| Grain | Bucket/project | Object or bucket |
| Enabled by default? | Always | Only if uniform bucket-level access is OFF |
| Use case | Standard, simpler management | Mixed public/private objects in same bucket |
| Revert window after enabling uniform access | 90 days | N/A |

### Cloud SQL Secure Access
- **IAM DB Authentication**: supported for MySQL & PostgreSQL; MySQL usernames truncate at 32 chars (gotcha).
- **Private IP**: requires VPC peering / Private Service Access; avoids public IP exposure entirely.
- **Cloud SQL Auth Proxy**: encrypts connection + handles IAM-based authorization without managing SSL certs manually.

### Secret Manager
| Item | Detail |
|---|---|
| Max payload size | 64 KiB per version |
| Version states | ENABLED, DISABLED, DESTROYED (irreversible) |
| Key IAM roles | admin, secretAccessor (read value), secretVersionManager (manage lifecycle, no read) |
| Replication: Automatic | Google chooses regions |
| Replication: User-managed | You specify exact region list — required for residency compliance |

### Exam Gotchas
- Authorized view must be in a **different dataset** than the source table.
- Uniform bucket-level access **disables ACLs completely** — mixed public/private objects need fine-grained (ACL) mode instead.
- CryptoHashConfig is NOT reversible — don't confuse with CryptoDeterministicConfig.
- MySQL IAM auth usernames truncate at 32 characters; PostgreSQL does not have this limit.
