## Chapter 5 Cheatsheet — VPC-SC, Shared VPC, Private Connectivity

### VPC Service Controls vs Firewall Rules
| Control | Layer | Protects Against |
|---|---|---|
| Firewall Rules | Network (IP/port) | Unauthorized network traffic between hosts |
| VPC-SC Perimeter | API/service | Data exfiltration from managed services (BigQuery, GCS, etc.) even with valid IAM |

- Perimeters group projects; Access Levels (Access Context Manager) define trusted identities/context (IP range, device policy).
- **Ingress rules**: let outside identities reach protected resources inside.
- **Egress rules**: let inside resources reach resources outside.
- **Dry-run mode**: logs violations without blocking — always test here first.
- Common exam trap: assuming firewall rules stop data copy to an external project — they don't; only VPC-SC does.

### Shared VPC vs VPC Peering
| Feature | Shared VPC | VPC Peering |
|---|---|---|
| Trust model | Centralized (host project owns network) | Decentralized (each side manages own) |
| Subnets | Shared — same subnet across projects | Separate subnets per VPC |
| Firewall/Routes | Controlled only by host project admin | Each side controls independently |
| Transitivity | N/A (single network) | Non-transitive |
| Overlapping CIDR | Not applicable (shared subnets) | Not allowed — connection fails |
| Key IAM role | roles/compute.xpnAdmin (host), roles/compute.networkUser (service project) | Standard project IAM per VPC |
| Default connection limit | N/A | 25 per network (increasable to 200) |

### Private Google Access vs Private Service Connect (PSC)
| Feature | Private Google Access | Private Service Connect |
|---|---|---|
| Purpose | Reach public Google APIs from VMs with no external IP | Privately consume a specific published service (Google or 3rd-party) via internal endpoint |
| Scope | Per-subnet setting | Endpoint object in consumer VPC |
| Cross-VPC/org | No (same network only) | Yes — across VPCs/orgs, no peering needed |
| CIDR overlap concern | N/A | None — single internal IP endpoint |

### Hybrid Connectivity
| Option | Bandwidth | Path | SLA |
|---|---|---|---|
| HA VPN (2x2 config) | Up to ~3 Gbps per tunnel | Public internet, encrypted | 99.99% |
| Dedicated Interconnect | 10 Gbps or 100 Gbps | Private physical circuit | 99.99% (redundant) |
| Partner Interconnect | 50 Mbps–50 Gbps | Private via partner | 99.9%–99.99% |

### Cloud NAT
- Outbound-only; no unsolicited inbound.
- Regional, fully managed — no gateway VMs to patch.
- Use for: private GKE nodes pulling public images, VMs without external IP needing general internet egress (non-Google APIs).

### Quick Decision Rules
- Need to stop data leaving to external GCS/BigQuery? → **VPC-SC perimeter + egress rules**.
- Need centralized network admin across teams? → **Shared VPC**.
- Need cross-org/VPC private access to one specific service? → **Private Service Connect**.
- Need VMs (no external IP) to reach only Google APIs? → **Private Google Access**.
- Need VMs (no external IP) to reach general internet (e.g., registries)? → **Cloud NAT**.
- Need dedicated 10G/100G private link to on-prem? → **Dedicated Interconnect**.
