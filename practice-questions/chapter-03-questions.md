## Chapter 3: App-ID and User-ID for Safe Application Enablement — Practice Questions

**Q1.** A firewall sits behind an explicit forward proxy. All web traffic arrives at the firewall with the proxy's source IP instead of the actual client IP, so User-ID cannot map users correctly. Which configuration change resolves this?

A) Deploy a Windows-based User-ID agent on the proxy server
B) Enable the Terminal Server agent on the firewall
C) Increase the User-ID timeout value in the User Mapping settings
D) Configure the firewall to trust and read the X-Forwarded-For (XFF) header from the proxy

<details>
<summary>Answer</summary>

**Correct: D**

When traffic is proxied, the real client IP is preserved only in the XFF HTTP header. Enabling XFF-based User-ID lets the firewall extract the original client IP from that header for accurate mapping.

**Why the others are wrong:**
- The Windows-based agent maps IP-to-user from AD logs; it doesn't solve the proxy IP masking problem
- TS agent solves shared-IP scenarios like RDS/Citrix, not proxy IP masking
- Adjusting timeout values doesn't address the underlying IP visibility problem

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** Several hundred employees connect through a single Windows Terminal Server, all sharing one source IP address. The security team needs per-user visibility and policy enforcement for this traffic. Which User-ID component should be deployed?

A) PAN-OS integrated (agentless) User-ID agent
B) Captive Portal in NTLM mode
C) Terminal Server (TS) agent
D) Group mapping via LDAP server profile

<details>
<summary>Answer</summary>

**Correct: C**

The TS agent installs on the terminal/RDS server and allocates unique port ranges per user session, allowing the firewall to distinguish individual users sharing a single source IP.

**Why the others are wrong:**
- The agentless agent maps IP-to-user from domain controllers but cannot split one IP into multiple users
- Captive Portal authenticates individual sessions but is not designed as the primary solution for shared-IP terminal server environments
- Group mapping only retrieves group membership, not IP-to-user identity

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** An administrator created a security rule using Application Override for a custom internal TCP application running on a non-standard port. Weeks later, malware traffic tunneled inside that same custom port is not flagged by any threat signature. What is the most likely cause?

A) The malware signature was not included in the latest content update
B) The firewall's App-ID cache became corrupted
C) The Terminal Server agent misclassified the port allocation
D) Application Override bypassed further App-ID identification and all Content-ID inspection for that session

<details>
<summary>Answer</summary>

**Correct: D**

Application Override forces the session to be tagged as the specified app at connection setup and skips further App-ID re-evaluation as well as all Content-ID scanning (AV, vulnerability protection, WildFire, URL filtering) — a well-known exam trap.

**Why the others are wrong:**
- Even with a current signature, Content-ID never runs on overridden traffic, so this isn't the primary cause
- App-ID cache corruption is not a real behavior tested on the exam
- The TS agent is unrelated to Application Override behavior

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** Which PAN-OS configuration is required, in addition to User-ID IP-to-user mapping, before group-based security policies can be enforced?

A) Enabling Captive Portal in redirect mode
B) A separate LDAP server profile configured under Group Mapping Settings
C) Enabling XFF header parsing
D) Configuring the Terminal Server agent

<details>
<summary>Answer</summary>

**Correct: B**

Group mapping is a distinct configuration from IP-to-user mapping. It requires its own LDAP server profile under Device > User Identification > Group Mapping Settings to pull group membership from the directory.

**Why the others are wrong:**
- Captive Portal authenticates users but does not provide group membership data
- XFF only recovers the real client IP behind a proxy, unrelated to group data
- The TS agent solves shared-IP mapping, not group membership retrieval

</details>

---

**Q5.** Which User-ID agent type runs natively inside PAN-OS and queries domain controllers directly, without requiring a separately installed Windows service?

A) Windows-based User-ID agent
B) Terminal Server agent
C) PAN-OS integrated (agentless) User-ID agent
D) Syslog-based User-ID listener

<details>
<summary>Answer</summary>

**Correct: C**

The PAN-OS integrated agent runs directly on the firewall (agentless from an external-server perspective) and polls domain controllers for security event logs without needing a separate Windows-based agent install.

**Why the others are wrong:**
- The Windows-based agent must be installed on a dedicated Windows server
- The TS agent maps shared-IP terminal server sessions, not general domain-controller polling
- Syslog listeners parse third-party device logs (e.g., VPN concentrators), not domain controller events directly

</details>

---

**Q6.** An administrator navigates to Device > User Identification and needs to enable pulling AD group membership for use in security policy. Which tab contains this configuration, separate from the User Mapping tab?

A) Group Mapping Settings
B) Connection Security
C) Terminal Services Agent
D) Captive Portal Settings

<details>
<summary>Answer</summary>

**Correct: A**

Group Mapping Settings is a dedicated tab in Device > User Identification, deliberately separated from User Mapping, and it uses its own LDAP server profile to sync group membership.

**Why the others are wrong:**
- Connection Security governs agent-to-firewall connection encryption, not group mapping
- Terminal Services Agent settings configure TS agent connections, unrelated to group sync
- Captive Portal Settings configure the active authentication redirect, not LDAP group data

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** Which dynamic content update category must be current for the firewall to correctly identify newly released applications via App-ID?

A) WildFire Signatures
B) Applications and Threats
C) PAN-DB URL Filtering
D) Antivirus

<details>
<summary>Answer</summary>

**Correct: B**

New and updated App-ID signatures are delivered through the Applications and Threats dynamic update (or Applications-only for devices without a Threat Prevention license).

**Why the others are wrong:**
- WildFire Signatures cover malware file/hash verdicts, not application identification
- PAN-DB URL Filtering updates URL category data, not App-ID signatures
- Antivirus updates cover AV signatures, unrelated to application identification

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** In what order does App-ID attempt to identify an application within a new session?

A) Pattern-based signatures, then protocol decoding, then heuristics for evasive apps
B) Heuristics, then protocol decoding, then pattern-based signatures
C) Protocol decoding only, since signatures are deprecated
D) Application Override, then signatures, then decoders

<details>
<summary>Answer</summary>

**Correct: A**

App-ID first applies pattern-based application signatures; if unresolved, it uses protocol decoders to identify known protocols and detect apps tunneling within them; heuristics are used last, specifically for evasive apps that resist both prior techniques.

**Why the others are wrong:**
- Heuristics are the last resort, not the first check, in the identification process
- Protocol decoding is one of three techniques, not the sole method — signatures are still primary
- Application Override is an administrator-configured policy action, not part of the automatic identification order

</details>

---

