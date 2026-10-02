## Chapter 9 Cheat Sheet — Binary Authorization, CVE Scanning, Security Posture

### Binary Authorization: evaluationMode
| Mode | Behavior |
|---|---|
| ALWAYS_ALLOW | No attestation check — everything deploys |
| ALWAYS_DENY | Blocks all deployments regardless of attestation |
| REQUIRE_ATTESTATION | Deploys only if valid attestor signature(s) present |

### Binary Authorization: enforcementMode
| Mode | Behavior |
|---|---|
| DRYRUN_AUDIT_LOG_ONLY | Logs violations, never blocks — use to test new policy |
| ENFORCED_BLOCK_AND_AUDIT_LOG | Blocks non-compliant deploys + logs |

### Break-Glass Bypass
| Platform | Mechanism |
|---|---|
| GKE | Pod annotation `alpha.image-policy.k8s.io/break-glass: "true"` |
| Cloud Run | Revision annotation `run.googleapis.com/breakglass: <justification>` |

Both are logged in Cloud Audit Logs — exam expects you to know bypass ≠ disabling the policy.

### Attestation Key Fact
Attestations sign the image **digest** (sha256), never the tag. Tags are mutable; digests are not.

### Supported Platforms for Binary Authorization
GKE ✅ · Cloud Run ✅ · GKE Enterprise (Anthos) ✅ · Cloud Functions ❌ (common distractor)

### CVSS v3 Severity Bands
| Severity | Score |
|---|---|
| LOW | 0.1–3.9 |
| MEDIUM | 4.0–6.9 |
| HIGH | 7.0–8.9 |
| CRITICAL | 9.0–10.0 |

### Vulnerability Scanning (Artifact Analysis / Container Analysis)
- On-push scan when image lands in Artifact Registry.
- Continuous rescanning of stored images as new CVEs are disclosed (no new push needed).
- Findings surfaced with CVSS severity + affected package.

### Security Command Center: Security Health Analytics
| Tier | Coverage |
|---|---|
| Standard | Limited, free detector set |
| Premium | Expanded detectors + compliance benchmark mappings (CIS, PCI, NIST) |

Detects misconfigurations: open firewalls, public buckets, missing MFA, non-org IAM members, etc. **Detective**, not preventive.

### Custom Organization Policy Constraints
Structure: `resourceTypes` + `methodTypes` (CREATE/UPDATE) + `condition` (CEL) + `actionType` (ALLOW/DENY).
Use when no predefined constraint covers the required guardrail (preventive control, unlike SHA which is detective).

### Image Hardening Checklist
- Prefer distroless or minimal base images (no shell/package manager = smaller attack surface).
- Patch VM base images regularly via OS Config patch management.
- Pin dependency versions; rebuild on CVE disclosure rather than patching running containers.

### Common Exam Traps
1. Confusing DRYRUN (never blocks) with ENFORCED (blocks) enforcementMode.
2. Assuming Binary Authorization supports Cloud Functions — it does not.
3. Thinking break-glass disables the policy — it's a logged, scoped bypass, not a policy change.
4. Attesting against a tag instead of digest — attestations must reference the digest.
