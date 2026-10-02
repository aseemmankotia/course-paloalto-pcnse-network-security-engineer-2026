## Chapter 6 Cheat Sheet: URL Filtering, WildFire, DNS Security

### URL Filtering Profile Actions
| Action | Behavior | Password/Click Required? |
|---|---|---|
| Allow | Session proceeds, no log | No |
| Alert | Session proceeds, logged | No |
| Block | Session denied, block page shown | No |
| Continue | Warning page, user must click through | Click only (no password) |
| Override | Block page requires password to bypass | Admin/help-desk password |
| None | Category not evaluated by this profile | N/A |

**Exam trap:** Continue ≠ Override. Continue = user click; Override = password entered (config'd under Device > Setup > Content-ID > URL Admin Override).

### WildFire File Size Limits (defaults, configurable up to platform max)
| File Type | Default Max Size |
|---|---|
| PE (executable) | 10 MB |
| APK (Android) | 30 MB |
| PDF | 3 MB |
| Archive (RAR/7z/ZIP) | 50 MB |

Configured at: **Device > Setup > WildFire**. The **WildFire Analysis security profile** (Objects > Security Profiles) controls *which* file types/zones/apps forward — not the size ceiling.

### WildFire Workflow
1. Firewall matches file to WildFire Analysis profile rule → forwards file (or hash lookup first).
2. Public cloud or WF-500 appliance performs static + dynamic sandbox analysis.
3. Verdict returned: **Benign / Grayware / Malware / Phishing**.
4. New signature generated and distributed globally (as fast as every 5 min with active subscription).
5. Logged in **WildFire Submissions log**; enforcement action logged separately in **Threat log** once signature is live.

### DNS Security Categories (Anti-Spyware profile > DNS Policies)
| Category | Meaning |
|---|---|
| Malware | Known malicious domains |
| Phishing | Credential-harvesting domains |
| Command and Control (C2) | Active malware callback infrastructure |
| Grayware | Adware/PUP-associated domains |
| DGA | Algorithmically generated domain names |
| Newly Registered Domains | Recently created domains (often abused) |
| Parked Domains | Inactive/placeholder domains |

**Action options per category:** Alert, Allow, Block, **Sinkhole** (redirects DNS response to a controlled IP to identify infected hosts).

### Key Log Locations
- Monitor > Logs > **URL Filtering** — category/action per URL request
- Monitor > Logs > **WildFire Submissions** — file hash + verdict
- Monitor > Logs > **Threat** — enforcement once signature/verdict is applied
- Monitor > Logs > **DNS Security** (subset of Threat log) — sinkhole/DNS category hits
