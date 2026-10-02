## Chapter 10: Cloud Audit Logs, Cloud IDS, and Security Command Center — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Which Cloud Audit Log types are always on and cannot be disabled? | Admin Activity, System Event, and Policy Denied. Exam tip: Data Access is the only opt-in type for most services (BigQuery Data Access is on by default). |
| 2 | Where do Admin Activity, System Event, and Policy Denied logs get stored, and for how long? | The immutable _Required log bucket, 400-day retention, free of charge. Exam tip: contrast with _Default bucket (30-day default) used for Data Access logs. |
| 3 | Default retention period for the _Default log bucket (holds Data Access logs)? | 30 days (configurable up to 3650 days on a custom bucket). Exam tip: enabling Data Access logs also incurs ingestion cost, unlike _Required bucket logs. |
| 4 | Which service has Data Access audit logs enabled by default, unlike almost every other GCP service? | BigQuery. All other services require you to explicitly enable Data Access logs per service in IAM > Audit Logs. |
| 5 | Log sink vs aggregated sink — key difference? | A log sink is scoped to a single project/folder/org resource. An aggregated sink is created at org or folder level with includeChildren=true, automatically capturing matching logs from all current and future child resources. |
| 6 | gcloud command pattern to create an org-level aggregated sink? | gcloud logging sinks create SINK_NAME DESTINATION --organization=ORG_ID --include-children --log-filter='...' — Exam tip: without --include-children, child project logs are NOT included even at org scope. |
| 7 | Security Command Center Standard tier — what's included? | Security Health Analytics (basic misconfiguration detection) at no cost. Does NOT include Event Threat Detection, Container Threat Detection, VM Threat Detection, or full Web Security Scanner. |
| 8 | What does SCC Premium add over Standard? | Event Threat Detection, Container Threat Detection, VM Threat Detection, full Web Security Scanner scans, and premium Security Health Analytics rules. Paid tier. |
| 9 | What does SCC Enterprise add over Premium? | Multicloud CSPM (AWS/Azure), case management, SOAR-style workflow integration, and Mandiant threat intel enrichment for findings. |
| 10 | How does Cloud IDS get access to traffic to inspect it? | Via a Packet Mirroring policy — Cloud IDS clones traffic out-of-band from VMs to a managed inspection endpoint using Palo Alto Networks signature-based detection. |
| 11 | VPC Flow Logs vs Firewall Rules Logging (Cloud NGFW logs) — what does each show? | VPC Flow Logs: 5-tuple flow metadata (src/dst IP, port, protocol, bytes) for network visibility. Firewall Rules Logging: which specific firewall rule allowed/denied a given connection. |
| 12 | What is Log Analytics used for? | Running BigQuery-style SQL queries directly against a log bucket's contents (after upgrading the bucket), enabling joins across log types without a separate BigQuery export. |
