## Chapter 4: Content-ID and SSL/TLS Decryption — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | SSL Forward Proxy certificate requirement? | Firewall needs a CA certificate (self-signed or subordinate) that must be imported into every client endpoint's trusted root store (GPO/MDM). Exam tip: cert trust failure = clients not trusting firewall's CA, not a server cert issue. |
| 2 | SSL Inbound Inspection certificate requirement? | The actual destination server's certificate AND private key must be imported onto the firewall. No client-side config needed since clients already trust the real cert. |
| 3 | What does Decryption Broker do? | Sends decrypted plaintext traffic from the firewall to one or more third-party security chain appliances (IPS, DLP, etc.) for inspection, avoiding redundant decryption at each device. |
| 4 | List the 5 core Content-ID components tested on PCNSE. | Threat Prevention (AV/Anti-Spyware/Vulnerability Protection), URL Filtering, File Blocking, Data Filtering, WildFire integration. |
| 5 | Order of Content-ID security profile scanning within a matched session? | Antivirus → Anti-Spyware → Vulnerability Protection → URL Filtering → File Blocking → Data Filtering → WildFire Analysis (for unknown files). |
| 6 | Decryption Profile: 'Block sessions with untrusted issuers' setting purpose? | Terminates the session if the certificate chain cannot be validated to a trusted root CA. A core certificate-chain-validation control in the SSL Decryption Profile. |
| 7 | When should you use a decryption policy-based exclusion? | For traffic that legally/technically cannot be decrypted — e.g., certificate-pinned mobile apps, or regulated financial/healthcare sites — while still allowing the session to match Security policy for logging/control. |
| 8 | What maintains the predefined SSL Decryption Exclusion list? | Palo Alto Networks maintains and updates it via content updates, covering known certificate-pinned applications that break under decryption. |
| 9 | Which PAN-OS version introduced native TLS 1.3 decryption support? | PAN-OS 10.0. Before this, TLS 1.3 sessions were downgraded or handled differently. Exam tip: know minimum version tied to decryption feature support. |
| 10 | Role of WildFire within Content-ID? | Analyzes unknown/suspicious files (submitted after AV/File Blocking checks) in a cloud or on-prem (WF-500) sandbox and returns a verdict, updating global signatures. |
| 11 | Data Filtering vs File Blocking — what's the difference? | File Blocking controls transfer based on file TYPE (e.g., block .exe uploads). Data Filtering inspects file CONTENT for defined data patterns (e.g., credit card numbers, SSNs) to prevent data exfiltration. |
| 12 | Default firewall behavior when a decrypted session presents an expired certificate (if the corresponding block setting is disabled)? | The session is allowed to proceed, but the firewall logs the certificate issue. Exam trap: many assume expired certs are auto-blocked — only true if 'Block sessions with expired certificates' is explicitly enabled. |
