## Chapter 3 Cheat Sheet — App-ID & User-ID

### App-ID Identification Order
| Step | Technique | Notes |
|---|---|---|
| 1 | Pattern-based signatures | Matches known app patterns regardless of port |
| 2 | Protocol decoding | Identifies known protocols and apps tunneling inside them (e.g., HTTP) |
| 3 | Heuristics | Last resort for evasive apps (P2P, tunneling, encrypted apps) |

**Exam trap:** App-ID signatures update via **Applications and Threats** (or **Applications-only** without Threat Prevention license) dynamic content updates — not a static, one-time database.

### Application Override — Risk Summary
| Behavior | Effect |
|---|---|
| App classification | Locked in at session setup as the overridden app |
| Further App-ID checks | Skipped |
| Content-ID (AV, vuln protection, WildFire, URL filtering) | Skipped entirely |

**Rule of thumb:** Only use Application Override for known, trusted custom apps — never as a shortcut around port-based rules.

### User-ID Agent Types
| Agent | Where it runs | Best for |
|---|---|---|
| Windows-based User-ID agent | Dedicated Windows server | Large/distributed AD environments |
| PAN-OS integrated (agentless) agent | On the firewall itself | Small/simple environments, no extra server |
| Terminal Server (TS) agent | On the RDS/Citrix server | Many users sharing one source IP |
| Captive Portal | Firewall (redirect) | Active auth fallback (NTLM/browser challenge/form) |
| XFF-based mapping | Firewall reads HTTP header | Users behind a forward proxy |

### Group Mapping vs User Mapping
| Config | Purpose | Location |
|---|---|---|
| User Mapping | IP-to-user mapping (server monitoring, client probing, agents) | Device > User Identification > User Mapping |
| Group Mapping | Group membership sync from directory | Device > User Identification > Group Mapping Settings (separate LDAP server profile) |

**Exam trap:** Group mapping and User mapping are configured independently — both are required for group-based security policy to work, but neither substitutes for the other.

### Quick Decision Guide
- Users behind a proxy → **Enable XFF**
- Shared IP (RDS/Citrix) → **TS agent**
- No dedicated server available → **PAN-OS integrated agent**
- Large AD environment, dedicated collector desired → **Windows-based agent**
- Passive mapping fails / unknown user → **Captive Portal**
- Need AD group in policy → **Configure Group Mapping Settings with LDAP profile**
