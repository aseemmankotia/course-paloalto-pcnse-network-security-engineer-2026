## Chapter 9: GlobalProtect, Certificate Management, and Logging — Practice Questions

**Q1.** A company deploys GlobalProtect for remote access. Users report that after successful authentication, their GlobalProtect app never displays a list of available gateways, though connectivity to the portal succeeds. Which component is most likely misconfigured?

A) The gateway's HIP profile
B) The certificate profile assigned to the gateway
C) The portal's agent configuration gateway list
D) The security policy allowing traffic to the gateway

<details>
<summary>Answer</summary>

**Correct: C**

The portal's sole responsibility is delivering the GlobalProtect app, client configuration, and the list of available gateways — a missing or misconfigured gateway list under the portal Agent config directly causes this symptom.

**Why the others are wrong:**
- A HIP profile affects post-connection policy enforcement, not whether a gateway list is displayed.
- A certificate profile on the gateway validates client certs during tunnel setup, not the portal's config push.
- Security policy controls traffic flow, not the delivery of configuration data from the portal.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** An administrator configures GlobalProtect with a split-tunnel setup, including only 10.0.0.0/8 in the Access Route list. A user connects and reaches internal servers fine, but browsing to public websites also appears to be tunneled, consuming corporate bandwidth. What is the most likely cause?

A) The portal agent configuration lacks HIP notification settings
B) The gateway's Access Route list also contains 0.0.0.0/0, forcing all traffic through the tunnel and overriding the intended split tunnel
C) The client certificate has expired, forcing fallback to full tunnel mode
D) The GlobalProtect app is running in Internal Host Detection mode instead of split tunnel mode

<details>
<summary>Answer</summary>

**Correct: B**

If 0.0.0.0/0 is present in the Access Route list alongside 10.0.0.0/8, the gateway pushes a default route into the tunnel, effectively tunneling all traffic regardless of the intended split configuration.

**Why the others are wrong:**
- HIP notifications only control endpoint pop-up messages, unrelated to traffic routing.
- An expired client certificate would fail the connection entirely, not silently switch to full tunnel.
- Internal Host Detection determines whether the client treats itself as internal or external for gateway selection, not tunnel routing scope.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A security team wants to block access to internal financial applications for any endpoint lacking a specific disk encryption product, enforced at the GlobalProtect gateway. Which combination correctly implements this?

A) Configure a HIP object matching the disk encryption vendor, add it to a HIP profile, and reference the HIP profile in a security policy rule restricting access to the internal app
B) Configure a certificate profile with the vendor's OID and enforce it in the SSL/TLS Service Profile
C) Create a Vulnerability Protection profile matching the disk encryption signature and attach it to the security policy
D) Configure the encryption requirement in the GlobalProtect Portal agent configuration under Network Services

<details>
<summary>Answer</summary>

**Correct: A**

HIP objects define host state criteria (e.g., disk encryption), HIP profiles group those objects, and referencing the HIP profile in a security policy rule is the standard mechanism to enforce host-compliance-based access at the gateway.

**Why the others are wrong:**
- Certificate profiles validate certificate chains, not endpoint software state — there is no OID-based encryption check here.
- Vulnerability Protection profiles inspect network traffic for exploits, not endpoint disk encryption status.
- The portal agent configuration delivers client settings but has no mechanism to enforce encryption compliance for app access.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** Which certificate should be imported into the firewall's trusted CA store to enable SSL Forward Proxy decryption without generating browser certificate warnings on managed endpoints?

A) A publicly-trusted commercial CA certificate installed only on the firewall
B) A self-signed certificate generated automatically by the firewall for each decrypted session
C) The device certificate used for GlobalProtect gateway authentication
D) An enterprise/internal intermediate CA certificate, issued by the organization's root CA, imported as a Forward Trust CA on the firewall, with the same chain distributed to endpoint trust stores via GPO/MDM

<details>
<summary>Answer</summary>

**Correct: D**

For SSL Forward Proxy, the firewall dynamically issues per-session certificates signed by an internal CA it holds the private key for. Endpoints must trust that CA chain (via GPO/MDM) to avoid browser warnings.

**Why the others are wrong:**
- Public CAs will not issue a certificate allowing arbitrary domain impersonation for forward proxy signing purposes.
- The firewall doesn't generate self-signed certs per session in a supported forward proxy design — it signs with an imported CA key.
- The device certificate authenticates the firewall to GlobalProtect/Panorama, unrelated to forward proxy decryption.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** An organization needs firewall traffic, threat, and WildFire logs delivered in real time to a third-party SIEM while also retaining full log data in Panorama for compliance. Which configuration meets this requirement?

A) Configure Panorama to relay all received logs to the SIEM via SNMP trap
B) Configure each firewall to send logs only to Panorama, then configure Panorama's Collector Group to forward logs to the SIEM via an email server profile
C) Configure log forwarding profiles on security policy rules to forward matched logs to both a syslog server profile and Panorama simultaneously
D) Configure only System and Config log forwarding, since traffic, threat, and WildFire logs cannot be forwarded to external destinations

<details>
<summary>Answer</summary>

**Correct: C**

A log forwarding profile attached to security policy rules can forward matched traffic/threat/WildFire logs to multiple destinations concurrently, including a syslog server profile and Panorama.

**Why the others are wrong:**
- SNMP trap is designed for lightweight alerting, not bulk real-time log delivery to a SIEM.
- Email server profiles are intended for alert notifications, not high-volume SIEM log ingestion.
- Traffic, threat, and WildFire logs can absolutely be forwarded externally via log forwarding profiles.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** An administrator configures a Certificate Profile with OCSP enabled for revocation checking on GlobalProtect client certificates. During testing, the OCSP responder is unreachable, and users cannot connect. Which single setting change allows connections to succeed while still validating certificates when revocation data IS available?

A) Enable CRL as an additional revocation method alongside OCSP
B) Uncheck 'Block Session If Certificate Status Is Unknown' in the Certificate Profile
C) Increase the Certificate Status Timeout value to 60 seconds
D) Remove the OCSP URL so the firewall skips revocation checks entirely

<details>
<summary>Answer</summary>

**Correct: B**

Unchecking 'Block Session If Certificate Status Is Unknown' allows sessions to proceed when revocation status cannot be determined (e.g., responder unreachable), while still enforcing revocation when the status IS known — this is the documented exam behavior for this checkbox.

**Why the others are wrong:**
- Adding CRL as a fallback still requires network reachability and doesn't directly solve the 'unknown status' blocking behavior by itself.
- Increasing the timeout may reduce false unreachable errors slightly but doesn't fundamentally change the block-on-unknown behavior.
- Removing the OCSP URL disables revocation checking entirely, which violates the requirement to retain validation.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** A log forwarding profile named 'SOC-Forward' is configured to forward Critical and High severity threat logs to a syslog server profile. Logs are not arriving at the syslog server despite matching traffic. What is the most likely missing configuration step?

A) The syslog server profile must be separately enabled under Device > Setup > Management
B) The log forwarding profile must be attached to the relevant security policy rules
C) The threat logs must be manually exported via CSV before forwarding takes effect
D) SNMP trap must be enabled globally before syslog forwarding will function

<details>
<summary>Answer</summary>

**Correct: B**

A log forwarding profile has no effect until it is referenced in the Actions tab of one or more security policy rules (or applied to default log settings for system/config logs) — it does not act globally on its own.

**Why the others are wrong:**
- Syslog server profiles are configured under Device > Server Profiles and don't require a separate enable step in Management settings.
- Manual CSV export is unrelated to the automated log forwarding mechanism.
- SNMP trap and syslog forwarding are independent mechanisms; one is not a prerequisite for the other.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** Which statement correctly differentiates the roles of a GlobalProtect portal and a GlobalProtect gateway?

A) The portal terminates VPN tunnels and enforces HIP-based security policy, while the gateway only distributes the app and client configuration
B) The portal distributes the GlobalProtect app, client configuration, and gateway list to endpoints, while the gateway terminates the actual VPN tunnel and enforces HIP-based policy
C) Both the portal and gateway independently terminate VPN tunnels and load-balance sessions between them
D) The portal only functions in agentless/clientless mode, while the gateway is required exclusively for the traditional GlobalProtect app

<details>
<summary>Answer</summary>

**Correct: B**

This is the exact role split tested on the exam: portal = configuration/distribution, gateway = tunnel termination and policy enforcement.

**Why the others are wrong:**
- This reverses the actual roles of portal and gateway.
- Only the gateway terminates VPN tunnels; the portal never does.
- Both portal and clientless VPN portal features can coexist with the traditional app; this statement incorrectly restricts portal function.

</details>

---

