## Chapter 10 Cheat Sheet: Structured Troubleshooting

### Packet Capture (pcap) Stages
| Stage | What it captures |
|---|---|
| receive | Packet as it arrives on ingress interface, before any policy processing |
| firewall | Packet after security policy/NAT/content processing, before egress queuing |
| transmit | Packet as it actually leaves the egress interface |
| drop | Packet discarded at ANY stage (policy deny, zone protection, etc.) |

**Workflow:** receive missing → ingress/interface issue. receive present but firewall missing → check policy/session/drop counters. firewall present but transmit missing → check egress routing/ARP/interface.

### Key CLI Test Commands
| Command | Purpose |
|---|---|
| `test security-policy-match source <ip> destination <ip> destination-port <p> protocol <n> from <zone> to <zone>` | Simulate security rule match, no live traffic needed |
| `test nat-policy-match ...` | Simulate NAT rule match and translated address |
| `test url-info-cloud <url>` / `test url-info-host <url>` | Check PAN-DB category (cloud = live, host = local cache) |
| `show session id <id>` | Inspect one live session: matched rule, NAT, app, flags |
| `show session all filter ...` | Search session table for matching flows |

### Traffic Log — Session End Reason Values
| Value | Meaning |
|---|---|
| policy-deny | Security rule denied/dropped the session |
| tcp-fin | Graceful TCP closure by endpoint |
| tcp-rst-from-client/server | Endpoint sent RST |
| aged-out | Idle timeout exceeded |
| decrypt-cert-validation | Server cert failed validation, Decryption Profile blocks |
| decrypt-unsupport-param | Unsupported cipher/protocol during SSL decryption |

**Isolating policy-match issues:** check `action` (allow/deny) + `rule` (matched rule name) + `session end reason` together — a mismatch between expected rule and logged rule = policy ordering/shadowing issue.

### Decryption Troubleshooting
- Check Decryption log (separate from Traffic log) for cert chain, cipher, and protocol failure detail.
- Common causes: expired/self-signed/untrusted CA cert, unsupported cipher suite, client doesn't trust Forward Trust CA (browser-side, not logged as session end reason).
- Decryption Profile settings control whether failures block or continue the session.

### HA Troubleshooting
| Link | Carries | Failure symptom |
|---|---|---|
| HA1 (control) | Heartbeats, state sync | Split-brain — both peers go active |
| HA2 (data) | Session table, config sync | Sessions not synced, but no split-brain alone |
| HA3 (active/active only) | Packet forwarding for asymmetric flows | Only relevant in active/active mode |

**Split-brain = HA1 down, both active.** First check: HA1 cabling, IP config, heartbeat/hello interval mismatch.

### Exam Traps
- Confusing `test security-policy-match` (security rules) with `test nat-policy-match` (NAT rules) — they test different rulebases.
- Assuming HA2 failure causes split-brain — it's HA1.
- Assuming `policy-deny` and `tcp-rst-from-client` mean the same thing — one is policy action, other is endpoint-driven.
- Using `show session all` to *predict* a match instead of `test security-policy-match` — session table only reflects existing traffic.
