## Chapter 5: Threat Prevention: Antivirus, Anti-Spyware, and Vulnerability Protection — Practice Questions

**Q1.** A company's internal DNS server resolves queries on behalf of all clients, so the firewall cannot see which internal host originally requested a malicious domain. Which feature lets the firewall still identify the infected host?

A) Enable DNS sinkholing in the Anti-Spyware profile
B) Enable URL Filtering block list for the domain
C) Set Vulnerability Protection critical severity to reset-both
D) Enable WildFire file analysis on the rule

<details>
<summary>Answer</summary>

**Correct: A**

DNS sinkholing rewrites the DNS response to a sinkhole IP, so the infected client's subsequent connection attempt to that sinkhole IP appears in traffic logs, revealing the true source host even when DNS was resolved by an internal server.

**Why the others are wrong:**
- URL Filtering blocks HTTP/HTTPS categories, not DNS resolution behavior, and doesn't solve the hidden-client problem.
- Vulnerability Protection matches exploit signatures in traffic, not DNS resolution redirection.
- WildFire analyzes files for malware; it has no role in identifying the source of a malicious DNS query.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** An admin applies the default Antivirus profile to a security rule. Infected files transferred over SMTP are only logged with an alert, while the same files downloaded over HTTP are blocked. Why?

A) The AV engine cannot scan SMTP traffic at all
B) The default Antivirus profile sets smtp/imap/pop3 decoders to alert and http/ftp/smb decoders to block by default
C) This indicates a licensing failure that must be reported to support
D) The Antivirus profile only inspects HTTP traffic unless manually configured otherwise

<details>
<summary>Answer</summary>

**Correct: B**

The built-in default Antivirus profile assigns different default actions per protocol decoder — email protocols (smtp, imap, pop3) default to alert to avoid disrupting mail delivery, while http, ftp, and smb default to block.

**Why the others are wrong:**
- The AV engine supports SMTP scanning; it simply defaults to a non-blocking action.
- This is expected default behavior, not a license error.
- The default profile scans multiple decoders, not only HTTP.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A security rule uses the default Vulnerability Protection profile. During testing, critical-severity exploit signatures only generate log alerts and are not dropped. What should the admin do to enforce blocking?

A) Modify the profile's critical/high severity rules to use an action such as reset-both or drop, or apply the predefined 'strict' profile
B) Nothing — the default profile already blocks all critical severity traffic
C) Attach an Anti-Spyware profile instead, since it handles exploit blocking
D) Enable File Blocking to compensate for the missing block action

<details>
<summary>Answer</summary>

**Correct: A**

Default profiles do not necessarily block every severity level; many rely on alert-only actions. To enforce blocking, the admin must set an explicit blocking action per severity or use the predefined 'strict' profile, which is tuned for stronger enforcement.

**Why the others are wrong:**
- This is the core exam gotcha — default profiles do not block all critical traffic automatically.
- Anti-Spyware detects spyware/C2 behavior, not exploit attempts against vulnerabilities.
- File Blocking filters by file type, not exploit signatures, and doesn't address this gap.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** Which security profile type matches network traffic against signatures for known exploits targeting software or OS vulnerabilities?

A) Antivirus
B) Vulnerability Protection
C) Anti-Spyware
D) URL Filtering

<details>
<summary>Answer</summary>

**Correct: B**

Vulnerability Protection profiles use exploit signatures to detect and block attempts to exploit known software or OS vulnerabilities.

**Why the others are wrong:**
- Antivirus scans files/streams for known malware signatures, not exploit attempts.
- Anti-Spyware detects spyware behavior and command-and-control traffic, not vulnerability exploitation.
- URL Filtering categorizes and controls web traffic by URL category, unrelated to exploit signatures.

</details>

---

**Q5.** Which object allows an administrator to bundle Antivirus, Anti-Spyware, Vulnerability Protection, URL Filtering, File Blocking, and WildFire Analysis profiles so they can be applied to a security rule with a single selection?

A) Security Profile Group
B) Application Override policy
C) Zone Protection Profile
D) HIP Profile

<details>
<summary>Answer</summary>

**Correct: A**

A Security Profile Group bundles multiple threat prevention and content profiles into one object, simplifying attachment to security rules.

**Why the others are wrong:**
- Application Override bypasses App-ID for specific traffic; it has no relation to profile bundling.
- Zone Protection Profiles defend against network-layer floods and reconnaissance at the zone level.
- HIP Profiles are used in GlobalProtect for endpoint posture assessment, unrelated to threat prevention profile bundling.

</details>

---

**Q6.** An Anti-Spyware signature is causing a false positive on a specific internal server's legitimate traffic, but the admin wants all other traffic to remain fully protected by that signature. What is the correct configuration approach?

A) Delete the Anti-Spyware profile from the rule entirely
B) Create a custom App-ID to replace the signature
C) Disable the security profile group on the rule
D) Add an exception for that specific signature ID in the profile's Exceptions tab and scope it to the affected IP address

<details>
<summary>Answer</summary>

**Correct: D**

Exception lists let an admin override the action for a specific signature ID, optionally scoped to particular source/destination IP addresses, tuning out false positives without weakening overall protection.

**Why the others are wrong:**
- Removing the profile eliminates all protection, not just the false positive.
- Custom App-ID controls application identification, not threat signature matching.
- Disabling the entire profile group removes all threat prevention protections from the rule.

</details>

---

**Q7.** What scanning technique does the Antivirus profile use to inspect files as they traverse the firewall?

A) Batch scanning after full file reassembly
B) Static signature scanning only after session close
C) Stream-based scanning
D) Sandbox-based dynamic analysis

<details>
<summary>Answer</summary>

**Correct: C**

The Antivirus profile uses stream-based scanning, inspecting file content as it passes through the firewall rather than waiting for the entire file to be buffered, minimizing latency.

**Why the others are wrong:**
- Batch scanning implies full buffering, which contradicts the stream-based design used to reduce latency.
- Scanning does not wait until session close; it occurs inline during transfer.
- Sandbox-based dynamic analysis describes WildFire, not the native Antivirus profile.

</details>

---

**Q8.** Which statement about the default (built-in) security profiles in PAN-OS is TRUE?

A) Default profiles apply varying actions per severity/protocol and do not block every severity level by default, often requiring tuning for a stricter posture
B) Default profiles block all severities across all threat types out of the box
C) Default profiles never block anything and must be entirely rebuilt from scratch
D) Default profiles cannot be edited once assigned to a policy rule

<details>
<summary>Answer</summary>

**Correct: A**

Default profiles assign different actions (often alert rather than block) depending on severity level and protocol decoder, which is a heavily tested exam gotcha requiring admins to tune profiles for stronger enforcement.

**Why the others are wrong:**
- Not all severities are blocked by default — some are only logged as alerts.
- Default profiles do provide baseline detection and some blocking; they are not entirely inert.
- Profiles remain fully editable at any time, even after being attached to a rule.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

