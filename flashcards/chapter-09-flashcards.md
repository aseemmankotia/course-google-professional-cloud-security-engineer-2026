## Chapter 9: Binary Authorization, CVE Scanning, and Security Posture Management — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Which compute platforms does Binary Authorization support? | GKE, Cloud Run, and GKE Enterprise (Anthos) clusters. NOT Cloud Functions. Exam tip: watch for Cloud Functions as a distractor option. |
| 2 | Does Binary Authorization sign the image tag or the image digest? | The digest (SHA-256). Tags are mutable and can be repointed, so attestations always bind to the immutable digest. |
| 3 | GKE break-glass annotation to bypass Binary Authorization? | alpha.image-policy.k8s.io/break-glass: "true" on the pod spec. The bypass is still recorded in Cloud Audit Logs. |
| 4 | Cloud Run break-glass annotation to bypass Binary Authorization? | run.googleapis.com/breakglass annotation with a justification string on the revision. Logged for audit review. |
| 5 | DRYRUN_AUDIT_LOG_ONLY vs ENFORCED_BLOCK_AND_AUDIT_LOG in Binary Authorization | DRYRUN logs policy violations but never blocks the deploy — used to safely test a new policy. ENFORCED blocks non-compliant deployments and logs them. Roll out new policies in DRYRUN first. |
| 6 | CVSS v3 severity score ranges | LOW 0.1–3.9, MEDIUM 4.0–6.9, HIGH 7.0–8.9, CRITICAL 9.0–10.0. |
| 7 | Does Artifact Analysis only scan on push? | No — it performs on-push scanning AND continuous rescanning of images already stored in Artifact Registry as new CVEs are published. |
| 8 | Where does Security Health Analytics live? | It's a built-in managed service inside Security Command Center that surfaces misconfiguration findings (open firewalls, public buckets, missing MFA) mapped to benchmarks like CIS. |
| 9 | SCC Standard vs Premium tier impact on Security Health Analytics | Standard tier includes a limited free set of Security Health Analytics detectors. Premium tier adds substantially more detectors plus compliance benchmark reporting (e.g., CIS, PCI, NIST). |
| 10 | What language/structure defines a custom Organization Policy constraint? | CEL (Common Expression Language) condition, plus resourceTypes, methodTypes (CREATE/UPDATE), and actionType (ALLOW/DENY) — used for guardrails not covered by predefined constraints. |
| 11 | Distroless image — what is deliberately missing? | No shell, no package manager, no unnecessary OS utilities — this shrinks the attack surface and removes common post-exploitation tools. |
| 12 | gcloud command family for managing Binary Authorization attestors? | gcloud container binauthz attestors create/list/describe to manage attestors; gcloud beta container binauthz attestations sign-and-create to generate a signed attestation for an image digest. |
