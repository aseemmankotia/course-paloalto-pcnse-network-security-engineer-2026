## Chapter 4: Content-ID and SSL/TLS Decryption — Practice Questions

**Q1.** A company wants to decrypt and inspect outbound HTTPS traffic from managed corporate laptops without triggering certificate trust warnings in browsers. What must be done on the firewall AND the endpoints for this to work correctly?

A) Generate a self-signed CA certificate on the firewall and push it to the endpoints' trusted root certificate store via GPO/MDM
B) Import the destination server's certificate and private key onto the firewall, then push it to endpoints via GPO
C) Enable SSL Inbound Inspection and configure a decryption policy-based exclusion for all internal subnets
D) Enable Decryption Broker so a third-party device signs certificates on behalf of the firewall

<details>
<summary>Answer</summary>

**Correct: A**

SSL Forward Proxy requires the firewall to generate/present a CA (or subordinate CA) certificate for re-signed sessions; that CA cert must be distributed and trusted by every client to avoid browser warnings.

**Why the others are wrong:**
- Importing the server's cert and key is required for Inbound Inspection, not Forward Proxy — the firewall never owns the real destination server's private key for outbound traffic
- Inbound Inspection is for protecting internal servers from external clients, not for outbound user browsing, and exclusions don't solve the trust-chain problem
- Decryption Broker forwards already-decrypted plaintext to third-party tools; it does not solve client trust of the firewall's re-signed certificate

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A firewall protects an internal web server accessible from the internet. Security wants to decrypt and inspect inbound sessions to that server for threats. What must be configured on the firewall?

A) Import the firewall's self-signed root CA certificate onto the internal web server
B) Configure SSL Forward Proxy and enable 'Block sessions with untrusted issuers'
C) Configure Decryption Broker to forward the session to the web server for re-signing
D) Import the web server's actual certificate and private key onto the firewall and configure SSL Inbound Inspection

<details>
<summary>Answer</summary>

**Correct: D**

SSL Inbound Inspection decrypts traffic destined for a server the organization owns. The firewall needs the server's real certificate and private key to transparently decrypt and re-encrypt the session without client trust issues, since clients already trust the real cert.

**Why the others are wrong:**
- The root CA import approach is backwards — that's used for Forward Proxy where clients trust the firewall's issuing CA, not for protecting an owned inbound server
- SSL Forward Proxy is for outbound client traffic to arbitrary external sites, not inbound traffic to an owned server
- Decryption Broker forwards decrypted plaintext to third-party security appliances; it does not perform the initial decryption itself

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** SSL Forward Proxy is enabled and a legally-required financial services site now fails to load because the vendor's mobile app uses certificate pinning. The security team must keep the traffic legally undecrypted while still logging it. What is the BEST configuration?

A) Disable SSL decryption globally to avoid all pinning issues
B) Import the site's real certificate into the firewall's trusted CA store
C) Add a decryption policy-based exclusion for the site/category so it bypasses decryption but still hits Security policy for logging
D) Enable Decryption Broker to forward the encrypted session directly to a third-party tool untouched

<details>
<summary>Answer</summary>

**Correct: C**

Decryption policy-based exclusions let specific traffic (by URL category, destination, or predefined SSL Decryption Exclusion list) bypass SSL decryption for compliance/technical reasons (e.g., pinned-cert apps) while the session is still matched, allowed, and logged by a Security policy rule.

**Why the others are wrong:**
- Disabling decryption globally removes visibility for all other traffic, far broader than required
- Importing the site's real cert into the trusted CA store has no effect on pinning issues and doesn't stop decryption attempts
- Decryption Broker deals with plaintext forwarding after decryption; it is unrelated to bypassing decryption for pinned apps

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** A customer has multiple third-party security appliances (IPS, DLP) that need to inspect decrypted plaintext copies of firewall traffic without performing their own SSL decryption. Which Palo Alto Networks feature should be used?

A) SSL Inbound Inspection
B) Decryption Broker
C) Decryption Mirroring
D) App-ID content updates

<details>
<summary>Answer</summary>

**Correct: B**

Decryption Broker allows the firewall to decrypt SSL/TLS traffic once and forward the decrypted plaintext to one or more third-party security chain devices for inspection, avoiding duplicate decryption overhead across devices.

**Why the others are wrong:**
- SSL Inbound Inspection decrypts traffic to an internally hosted server; it does not forward plaintext to external devices
- Decryption Mirroring sends a copy of decrypted traffic to a passive tool (e.g., packet capture) but does not chain traffic through inline security devices in sequence
- App-ID content updates are signature packages for application identification and have nothing to do with plaintext forwarding

</details>

---

**Q5.** Which Content-ID component is responsible for submitting unknown files to a cloud-based sandbox for analysis and returning a verdict to the firewall?

A) URL Filtering
B) Data Filtering
C) File Blocking
D) WildFire Integration

<details>
<summary>Answer</summary>

**Correct: D**

WildFire integration submits unknown or suspicious files to the WildFire cloud (or a WF-500 private appliance) for dynamic/static analysis and delivers a malware verdict back to the firewall, updating signatures globally.

**Why the others are wrong:**
- URL Filtering categorizes and controls access to websites based on URL category, not file analysis
- Data Filtering matches file content against data patterns (e.g., credit card, SSN) to prevent data exfiltration, not malware sandboxing
- File Blocking controls which file types can be uploaded/downloaded based on policy, but performs no sandbox analysis itself

</details>

---

**Q6.** In a Decryption Profile, which setting causes the firewall to terminate a session when the certificate chain cannot be validated to a trusted root?

A) Block sessions with expired certificates
B) Block sessions with unsupported protocol versions
C) Block sessions with untrusted issuers
D) Block sessions with client authentication

<details>
<summary>Answer</summary>

**Correct: C**

'Block sessions with untrusted issuers' in the SSL Decryption Profile enforces certificate chain validation, dropping sessions whose certificate cannot be traced to a CA the firewall trusts — a core exam topic for decryption profile hardening.

**Why the others are wrong:**
- This setting blocks sessions where the cert's validity dates have lapsed, unrelated to chain trust
- This setting blocks outdated/insecure TLS/SSL protocol versions (e.g., SSLv3), not untrusted issuers
- Client authentication blocking relates to mutual TLS scenarios, not CA trust validation

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** Within a Security Profile Group applied to a matching session, in which order does the firewall apply Content-ID scanning components?

A) URL Filtering, File Blocking, Antivirus, Data Filtering, Vulnerability Protection, WildFire
B) WildFire Analysis, Antivirus, URL Filtering, Vulnerability Protection, File Blocking, Data Filtering
C) File Blocking, Data Filtering, URL Filtering, WildFire Analysis, Antivirus, Vulnerability Protection
D) Antivirus, Anti-Spyware, Vulnerability Protection, URL Filtering, File Blocking, Data Filtering, WildFire Analysis

<details>
<summary>Answer</summary>

**Correct: D**

Palo Alto Networks documents a fixed order of operations for security profiles: Antivirus, Anti-Spyware, Vulnerability Protection, URL Filtering, File Blocking, Data Filtering, then WildFire Analysis submission for unknown files.

**Why the others are wrong:**
- Places URL Filtering and File Blocking before Antivirus, which is reversed from the documented processing order
- Places WildFire Analysis first, but WildFire submission for unknown files occurs last in the profile order, after other scans complete
- Reverses the entire order, starting with File/Data Filtering before the AV/Anti-Spyware/Vulnerability scans occur

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** What is the fundamental difference between SSL Forward Proxy and SSL Inbound Inspection in terms of certificate handling?

A) Forward Proxy requires no certificates; Inbound Inspection requires a CA certificate trusted by clients
B) Both require the same server certificate and private key imported onto the firewall
C) Forward Proxy decrypts inbound traffic to internal servers; Inbound Inspection decrypts outbound traffic from clients
D) Forward Proxy uses a firewall CA certificate trusted by clients to re-sign sessions; Inbound Inspection uses the real server's certificate and private key

<details>
<summary>Answer</summary>

**Correct: D**

Forward Proxy re-signs outbound sessions using a firewall-held CA certificate that clients must trust; Inbound Inspection uses the actual destination server's certificate and private key so no client-side trust configuration is needed.

**Why the others are wrong:**
- Both decryption modes require certificate configuration; neither is certificate-free, and the CA-trust requirement belongs to Forward Proxy, not Inbound Inspection
- The certificate requirements are opposite for each mode — they do not share the same certificate/key combination
- This reverses the actual traffic direction each mode is designed for: Forward Proxy is for outbound client traffic, Inbound Inspection is for inbound traffic to owned servers

</details>

---

