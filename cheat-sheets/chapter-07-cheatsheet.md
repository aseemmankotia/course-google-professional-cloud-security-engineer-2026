## Chapter 7 Cheat Sheet: CMEK, Cloud EKM, Confidential Computing

### Encryption at Rest — Decision Tree
| Option | Key Location | Customer Effort | Use When |
|---|---|---|---|
| Default (Google-managed) | Google infra | None | No compliance mandate on key control |
| CMEK (Cloud KMS) | Google infra (SOFTWARE/HSM) | Manage rotation, IAM, disable/destroy | Need audit trail, rotation control, revocation |
| Cloud EKM | External partner (outside Google) | Manage external KMS + connectivity | Regulatory mandate: key must never touch Google infra |
| BYOK (Import Job) | Google infra (imported bytes) | Wrap & import existing key | Must reuse specific existing key material |

### Cloud KMS Protection Levels
| Level | Description |
|---|---|
| SOFTWARE | Standard software-protected keys |
| HSM | Google-managed FIPS 140-2 Level 3 HSM |
| EXTERNAL | Cloud EKM, public internet path |
| EXTERNAL_VPC | Cloud EKM, private VPC/service-attachment path |

### Key Lifecycle Numbers
| Item | Value |
|---|---|
| Min automatic rotation period | 24 hours |
| Suggested default rotation | 90 days |
| Key version destroy default window | 24 hours |
| Destroy window configurable range | ~24h – 120 days |
| Disable | Reversible, immediate stop of use |
| Destroy | Irreversible after scheduled window |

### BYOK Import Flow
1. Generate/export existing key material.
2. Wrap with RSA-OAEP + AES-256 key wrap (e.g., RSA_OAEP_3072_SHA256_AES_256).
3. Create import job in Cloud KMS.
4. Import wrapped key into a key version — original bytes preserved.

### Confidential Computing
| Feature | Protects | Mechanism |
|---|---|---|
| Confidential VM | Data in use (RAM) | AMD SEV/SEV-SNP or Intel TDX memory encryption |
| Confidential GKE Nodes | Node memory in GKE | Same hardware tech applied to node VMs |
| CMEK / Default | Data at rest | AES-256 disk/object encryption |

### Key gcloud Commands
```bash
# Create key with rotation
gcloud kms keys create KEY --location=LOC --keyring=RING \
  --purpose=encryption --rotation-period=90d

# Disable a key version (reversible)
gcloud kms keys versions disable VER --key=KEY --keyring=RING --location=LOC

# Destroy a key version (irreversible after window)
gcloud kms keys versions destroy VER --key=KEY --keyring=RING --location=LOC
```

### Exam Traps
- Confusing **disable** (reversible) with **destroy** (irreversible) — exam loves this.
- Assuming CMEK removes the key from Google infra — it doesn't; only **Cloud EKM** does.
- Thinking CMEK/default encryption protects data **in use** — only Confidential Computing does.
- Forgetting min rotation period is 24 hours, not less.
