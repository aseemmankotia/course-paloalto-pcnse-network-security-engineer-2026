## Chapter 4 Cheat Sheet: Content-ID & SSL/TLS Decryption

### Decryption Modes — Certificate Requirements
| Mode | Use Case | Certificate Needed | Client Config Needed? |
|---|---|---|---|
| SSL Forward Proxy | Outbound client traffic to arbitrary internet sites | Firewall CA cert (self-signed or subordinate) | YES — CA cert must be pushed to endpoint trust store (GPO/MDM) |
| SSL Inbound Inspection | Inbound traffic to internally owned servers | Real server certificate + private key imported to firewall | NO — clients already trust the real cert |

**Exam trap:** Swapping these two — Forward Proxy needs a *CA* cert trusted by clients; Inbound Inspection needs the *actual server's* cert+key, not a CA.

### Content-ID Components
| Component | Function |
|---|---|
| Threat Prevention (AV/Anti-Spyware/Vuln Protection) | Signature-based malware, C2, and exploit detection |
| URL Filtering | Category/reputation-based web access control |
| File Blocking | Blocks/allows based on file type |
| Data Filtering | Pattern matching on file content (SSN, CC#, custom patterns) |
| WildFire Integration | Cloud/appliance sandboxing for unknown files, returns verdict |

### Content-ID Processing Order (Security Profile Group)
1. Antivirus
2. Anti-Spyware
3. Vulnerability Protection (IPS)
4. URL Filtering
5. File Blocking
6. Data Filtering
7. WildFire Analysis (unknown files only)

### Decryption Profile Key Settings
| Setting | Effect |
|---|---|
| Block sessions with untrusted issuers | Terminates session if cert chain fails validation to trusted root |
| Block sessions with expired certificates | Terminates session if server/client cert has expired |
| Block sessions with unsupported cipher suites | Blocks weak/unsupported ciphers from completing handshake |
| Block sessions with unsupported protocol versions | Blocks outdated SSL/TLS versions (e.g., SSLv3, TLS 1.0/1.1 if configured) |

### Decryption Exclusions
- **Policy-based decryption exclusion:** Manually configured rule to bypass decryption for specific URL categories/destinations (compliance: healthcare, financial; technical: cert pinning).
- **Predefined SSL Decryption Exclusion List:** Maintained by Palo Alto Networks via content updates; covers known certificate-pinned apps automatically.
- Excluded traffic still matches Security policy and is logged — only decryption is skipped.

### Decryption Broker
- Sends decrypted plaintext to third-party security chain devices (IPS/DLP/etc.) inline, in sequence.
- Avoids redundant decrypt/re-encrypt cycles across multiple inspection devices.
- Distinct from **Decryption Mirroring**, which sends a passive copy of decrypted traffic to a monitoring tool (not inline chained).

### Version Note
- Native TLS 1.3 decryption support: **PAN-OS 10.0+**.

### Common Exam Traps
1. Confusing Forward Proxy CA cert requirement with Inbound Inspection server cert requirement.
2. Assuming decryption exclusions stop policy enforcement entirely (they only bypass decryption).
3. Assuming expired/untrusted certs are blocked by default — they are only blocked if the relevant Decryption Profile checkbox is explicitly enabled.
