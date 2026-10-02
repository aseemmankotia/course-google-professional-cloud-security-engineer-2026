## Chapter 10 Cheat Sheet — Audit Logs, IDS, SCC

### Cloud Audit Log Types
| Type | Default State | Bucket | Retention | Cost |
|---|---|---|---|---|
| Admin Activity | Always on | _Required | 400 days | Free |
| System Event | Always on | _Required | 400 days | Free |
| Policy Denied | Always on (excludable via filter) | _Required | 400 days | Free |
| Data Access | Opt-in (BigQuery on by default) | _Default | 30 days (default) | Billed on ingestion |

**Memory trick:** "AASP" — Admin Activity, always on; System event, always on; Policy denied, always on; Data access, opt-in.

### Log Sinks
| Feature | Log Sink | Aggregated Sink |
|---|---|---|
| Scope | Single project/folder/org | Org or folder, spans children |
| Future projects | Not covered | Covered if includeChildren=true |
| Use case | One project's export | Centralized SIEM export org-wide |

```
gcloud logging sinks create NAME DEST \
  --organization=ORG_ID --include-children \
  --log-filter='logName:"cloudaudit.googleapis.com"'
```

### Security Command Center Tiers
| Tier | Key Finding Sources | Cost |
|---|---|---|
| Standard | Security Health Analytics (basic) | Free |
| Premium | + Event Threat Detection, Container Threat Detection, VM Threat Detection, Web Security Scanner | Paid |
| Enterprise | + Multicloud CSPM (AWS/Azure), case management, Mandiant intel | Paid |

### Network Visibility Tools
| Tool | Captures | Best For |
|---|---|---|
| VPC Flow Logs | 5-tuple flow metadata | Traffic pattern analysis, cost-effective |
| Firewall Rules Logging (Cloud NGFW) | Rule match allow/deny events | Troubleshooting firewall behavior |
| Packet Mirroring | Full packet clones, out-of-band | Feeds Cloud IDS or 3rd-party tools |
| Cloud IDS | Signature-based threat detection (Palo Alto) | Malware/C2/exploit detection |

### Exam Gotchas
- Data Access logs for most services are OFF by default; BigQuery is the exception (ON by default).
- Policy Denied logs cannot be fully disabled — only trimmed via exclusion filters.
- Aggregated sink requires `--include-children` to actually capture child project/folder logs.
- SCC Standard has NO Event/Container/VM Threat Detection — common wrong-answer trap when a scenario mentions "free tier" but expects premium findings.
- Cloud IDS requires Packet Mirroring infrastructure; it does not inspect traffic inline.
- Log Analytics runs SQL in-place on a log bucket — does not require a separate BigQuery export step (though linking to BigQuery is also possible).
