## Chapter 8 Cheat Sheet — Upgrades, HA, Config Management

### PAN-OS Upgrade Path Rule
| Rule | Detail |
|---|---|
| Sequential major releases | Cannot skip a major version's base image (e.g., 10.1 → 10.2 base → 11.0 base → 11.1) |
| Base image required | Install the base (x.x.0) image of each major release before jumping to a later maintenance release |
| Read release notes | Exam expects awareness of downgrade limitations and content version compatibility |

### HA Links Comparison
| Link | Mode | Purpose |
|---|---|---|
| HA1 (control) | Both A/P & A/A | Heartbeats, hellos, config/route sync — no session data |
| HA1-backup | Optional | Redundant control path |
| HA2 (data) | Both A/P & A/A | Session table, forwarding table, IPsec SA sync |
| HA3 (packet forwarding) | A/A only | Forwards packets for asymmetric traffic between peers |

### HA Election & Preemption
- **Priority:** lower numeric value = higher priority = active.
- **Preemption:** disabled by default; must enable on BOTH peers for automatic active-role reclaim after recovery.
- **Without preemption:** recovered device rejoins as passive; current active stays active.

### Active/Passive vs Active/Active
| Feature | Active/Passive | Active/Active |
|---|---|---|
| Traffic processing | One device at a time | Both devices concurrently |
| Extra HA link | Not needed | HA3 required |
| Session roles | N/A | Session owner + session setup roles |
| Complexity | Lower | Higher (asymmetric routing handling) |

### Config Management
| Term | Meaning |
|---|---|
| candidate-config.xml | Uncommitted pending changes |
| running-config.xml | Last committed, active config |
| Commit | Copies candidate → running |
| Export named config snapshot | Device > Setup > Operations — creates restorable backup |

### Content Update Best Practice
| Update Type | Recommended Practice |
|---|---|
| Applications and Threats | Download immediately, install with delay threshold (≥1 hr) |
| Antivirus | Frequent scheduled updates; low false-positive risk |
| WildFire | Near real-time signature delivery; shortest interval available |

**Exam gotchas:**
- Skipping a major release during upgrade = automatic wrong answer.
- 'Lower priority number wins' is reversed by most test-takers — memorize it.
- HA3 only exists in active/active — a question mentioning active/passive with HA3 packet forwarding is a distractor.
- Commit ≠ backup; only an exported named snapshot is a true restorable backup.
