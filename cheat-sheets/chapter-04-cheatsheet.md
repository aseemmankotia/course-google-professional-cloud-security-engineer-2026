## Chapter 4 Cheat Sheet — Perimeter Controls

### Firewall Evaluation Order (lowest priority number wins within a tier)
| Order | Tier | Priority Range | Notes |
|---|---|---|---|
| 1 | Hierarchical FW policy (Org) | 0–2147483647 | Evaluated first |
| 2 | Hierarchical FW policy (Folder) | 0–2147483647 | Evaluated after org |
| 3 | Network firewall policy (global/regional) | 0–2147483647 | Attached to VPC |
| 4 | VPC firewall rules (legacy) | 0–65535 (default 1000) | Evaluated last |

Explicit `allow`/`deny` rule (no `goto_next`) stops evaluation — lower tiers never checked. `goto_next` lets evaluation continue downward.

### Cloud NGFW Tiers
| Tier | Adds |
|---|---|
| Essentials | L3–L4 rules, tags, service accounts |
| Standard | FQDN objects, geo-IP filtering |
| Enterprise | IPS (signature-based), TLS inspection, threat intel |

### IAP Essentials
- Web app access: requires External Application LB (global, HTTPS), IAP toggle on backend service
- TCP forwarding (SSH/RDP): **no LB needed**; must allow ingress from **35.235.240.0/20**
- Context-aware access via Access Context Manager (device state, IP, geography, MFA)

### Cloud Armor
| Feature | Standard | Managed Protection Plus |
|---|---|---|
| WAF preconfigured rules (OWASP CRS) | ✅ | ✅ |
| Rate limiting (throttle/rate_based_ban) | ✅ | ✅ |
| Adaptive Protection (ML anomaly detection) | ❌ | ✅ |
| Named IP lists, enhanced DDoS support | ❌ | ✅ |

- **Edge security policy**: enforced at Cloud CDN edge, protects cached responses.
- **Backend security policy**: enforced at backend service.
- Rate limit actions: `throttle` (cap rate), `rate_based_ban` (temp ban via `ban_duration_sec` after exceeding threshold).

### Certificate Authority Service (CAS)
| Tier | Use case |
|---|---|
| DevOps | Short-lived certs, high automation, no CRL |
| Enterprise | Full PKI, CRL support, longer validity |

### Secure Web Proxy vs Cloud NAT
| | SWP | Cloud NAT |
|---|---|---|
| Layer | L7 (FQDN/URL filtering) | L3/L4 (address translation) |
| Managed proxy VMs needed | No | N/A |
| Use case | Allowlist outbound domains | Give private VMs outbound internet access |

### Cloud DNS Security
- **DNSSEC**: signs zone (RRSIG/DNSKEY) → authenticates responses, prevents spoofing/cache poisoning. Does NOT encrypt queries or stop DDoS.
- **Private zones**: resolvable only within specified VPC networks; combine with DNS forwarding/peering for hybrid setups.

### Key gcloud Commands
```
gcloud compute firewall-policies create --organization=ORG_ID --short-name=NAME
gcloud compute firewall-policies rules create --action=allow|deny|goto_next --firewall-policy=NAME
gcloud compute security-policies create --organization=ORG_ID  # Cloud Armor
gcloud compute security-policies rules create --action=throttle|rate-based-ban
gcloud privateca subordinates create ...  # CAS
```
