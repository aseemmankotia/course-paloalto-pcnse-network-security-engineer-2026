## Chapter 5 Cheat Sheet: AV, Anti-Spyware, Vulnerability Protection

### Antivirus Profile
| Item | Detail |
|---|---|
| Scan method | Stream-based (no full buffering) |
| Default action: http/ftp/smb | block |
| Default action: smtp/imap/pop3 | alert |
| Exam gotcha | Email protocols default to alert, NOT block |

### Anti-Spyware Profile
| Item | Detail |
|---|---|
| Purpose | Detects spyware behavior & C2 traffic |
| Severities | Critical, High, Medium, Low, Informational |
| DNS Sinkholing location | Anti-Spyware profile > DNS Signatures/DNS Policies tab |
| Sinkhole purpose | Redirects malicious DNS response to sinkhole IP so infected host is identifiable in traffic logs, even behind internal DNS resolvers |
| Exam gotcha | Default profile severities often set to alert only, not block |

### Vulnerability Protection Profile
| Item | Detail |
|---|---|
| Purpose | Matches exploit signatures against known software/OS vulnerabilities |
| Severities | Critical, High, Medium, Low, Informational |
| Predefined profiles | 'default' (lenient) and 'strict' (tuned for blocking) |
| Exam gotcha | Default profile does NOT block all critical exploits — verify/set actions explicitly |

### Available Actions (Anti-Spyware & Vulnerability Protection)
`default | allow | alert | drop | reset-client | reset-server | reset-both | block-ip`

### Security Profile Groups
- Bundle: Antivirus, Anti-Spyware, Vulnerability Protection, URL Filtering, File Blocking, WildFire Analysis (+ Data Filtering)
- Attached as a single object to a Security Policy rule
- Only takes effect on rules with action = **allow**

### Exception Lists
- Located within each profile (AV, Anti-Spyware, Vulnerability Protection)
- Override the action for a specific signature ID
- Can be scoped to specific source/destination IP addresses to fix false positives without weakening global protection

### Key Exam Themes
1. Default profiles vary action by protocol/severity — they do **not** block everything.
2. DNS Sinkholing is configured within Anti-Spyware, not URL Filtering or Vulnerability Protection.
3. Security Profile Groups simplify applying multiple threat profiles to a rule in one step.
4. Always verify severity-to-action mapping when a scenario says threats are only being logged, not blocked — the fix is usually adjusting profile actions or switching to 'strict'.
5. Use exceptions, not profile removal, to tune specific false-positive signatures.
