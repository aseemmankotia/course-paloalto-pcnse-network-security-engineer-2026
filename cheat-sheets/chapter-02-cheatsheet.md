## Chapter 2 Cheat Sheet — Security Policy, NAT, Zones, Interfaces

### Security Policy Match Criteria
| Field | Notes |
|---|---|
| Zone (src/dst) | Post-NAT zone used |
| Address (src/dst) | Pre-NAT IP used (EXCEPT dest-NAT: use post-NAT dest IP) |
| Application | App-ID; may trigger policy re-eval mid-session |
| Service/Port | TCP/UDP port match |
| Action | allow/deny; first match wins (top-down) |

**Rule Shadowing:** broad rule above specific rule = specific rule unreachable. Always order specific → general.

### NAT Types
| Type | Direction | Behavior |
|---|---|---|
| Dynamic IP and Port (PAT) | Outbound | Many-to-one, port multiplexed |
| Dynamic IP (no PAT) | Outbound | One-to-one from pool, no port translation; drops if pool exhausted (no fallback) |
| Static NAT | Bi-directional | Persistent 1:1 mapping, common for published servers |
| Destination NAT | Inbound | Translates dest IP/port (port forwarding); security rule uses POST-NAT dest IP |
| U-turn (Hairpin) NAT | Internal-to-internal | Combines src NAT + dst NAT so internal clients reach internal server via public IP |

### NAT + Security Policy Evaluation Order
1. NAT policy evaluated first → determines translation + egress interface/zone
2. Route/forwarding lookup on post-NAT destination (unless PBF overrides)
3. Security policy evaluated using **pre-NAT IPs + post-NAT zones**
4. Destination NAT exception: security rule dest address = post-NAT (translated) IP

### Interface Types
| Type | Use Case |
|---|---|
| Layer 3 Subinterface | 802.1Q VLAN tagging (1–4094), splits 1 physical port into multiple zones/VRs |
| Aggregate Ethernet (AE) | Bundles physical interfaces for bandwidth/redundancy (LACP) |
| Loopback | Mgmt access, VPN termination, NAT address; no physical link, still needs zone |
| Tap | Passive monitoring only, no Layer 3 traffic |

### Policy-Based Forwarding (PBF)
- Overrides routing table for matched traffic (zone/address/app/user based)
- Evaluated before standard forwarding lookup
- Common use: force specific app/traffic out a non-default link
- Can configure monitoring to fail over if PBF next-hop unreachable

### Exam Gotchas
- Security policy matches **pre-NAT IP** but **post-NAT zone** — classic trick question
- Destination NAT is the exception: use post-NAT dest IP in the rule
- Rule shadowing: broad rule above specific = silent failure
- Dynamic IP (no PAT) pool exhaustion = drop by default, not auto-PAT fallback
- U-turn NAT requires correct zone handling since source and destination zone may appear identical (internal-to-internal)
