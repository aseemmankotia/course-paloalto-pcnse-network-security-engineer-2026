## Chapter 6: Advanced URL Filtering, WildFire, and DNS Security — Practice Questions

**Q1.** A company wants users who click a link to an uncategorized 'gambling' site to see a warning page and be required to explicitly acknowledge it before the page loads, while still logging the event. Which URL Filtering profile action should be applied to that category?

A) Block
B) Alert
C) Continue
D) Override

<details>
<summary>Answer</summary>

**Correct: C**

Continue displays a response page that requires the end user to click through before the session is allowed to proceed, and the click-through event is logged in the URL Filtering log.

**Why the others are wrong:**
- Block denies the request outright — no response page allowing access exists, so users could never proceed.
- Alert only logs the visit and displays no interactive response page; the user is never required to acknowledge anything.
- Override requires an administrator-defined password to bypass the block page, not a simple end-user click-through.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A help desk needs a mechanism where end users can request temporary access to a blocked category, but only after a help desk agent enters a shared password on the response page. Which URL Filtering action provides this behavior?

A) Override
B) Continue
C) Alert
D) Allow

<details>
<summary>Answer</summary>

**Correct: A**

Override requires a password configured under Device > Setup > Content-ID (URL Admin Override) to be entered before granting temporary access, matching the help-desk-controlled workflow described.

**Why the others are wrong:**
- Continue only requires the end user to click a button — no password is involved.
- Alert takes no action beyond logging; there is no interactive gate at all.
- Allow permits the category with no response page or logging interaction whatsoever.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** An engineer needs the firewall to detect a brand-new, never-before-seen executable and generate a verdict based on dynamic sandbox behavior rather than a static signature database. Which security profile/service should be applied to the traffic?

A) WildFire Analysis profile
B) Antivirus profile
C) Vulnerability Protection profile
D) URL Filtering profile

<details>
<summary>Answer</summary>

**Correct: A**

WildFire forwards unknown files matching the profile's file type rules to the WildFire cloud (or WF-500 appliance) for dynamic sandbox analysis, returning a verdict (benign, grayware, malware, phishing) and generating new signatures.

**Why the others are wrong:**
- Antivirus profile only matches known signatures already present in the antivirus content update; it cannot analyze zero-day files.
- Vulnerability Protection targets exploit attempts against known CVEs in network traffic, not file content analysis.
- URL Filtering evaluates web categories/reputations of URLs, not file behavior.

</details>

---

**Q4.** A firewall administrator needs to increase the maximum file size forwarded to WildFire for PE (portable executable) files because larger installers are currently bypassing analysis. Where is this setting configured?

A) Objects > Security Profiles > WildFire Analysis > file type rule
B) Device > Setup > WildFire > file size limits
C) Objects > Security Profile Groups
D) Network > Zone Protection Profile

<details>
<summary>Answer</summary>

**Correct: B**

Per-file-type maximum forwarding sizes (PE, APK, PDF, archives, etc.) are configured under Device > Setup > WildFire, separate from the WildFire Analysis profile that decides which file types/applications get forwarded.

**Why the others are wrong:**
- The WildFire Analysis security profile controls which file types/applications/zones are forwarded and to which cloud (public or private), but not the size ceiling per file type.
- Security Profile Groups only bundle existing profiles together for attachment to a rule; they contain no size settings.
- Zone Protection Profiles defend against network floods and reconnaissance — unrelated to file analysis sizing.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** A SOC wants to identify which internal hosts are infected with malware by redirecting DNS responses for known-malicious domains to a controlled IP address that the firewall can then log and alert on. Which DNS Security feature accomplishes this?

A) DNS sinkholing
B) URL Filtering override
C) WildFire file blocking
D) Vulnerability Protection blocking

<details>
<summary>Answer</summary>

**Correct: A**

Sinkhole action, configured in the Anti-Spyware profile's DNS Policies, rewrites the DNS response for malicious domains to a sinkhole IP so the firewall can trace and log the internal host that later tries to connect to it.

**Why the others are wrong:**
- URL Filtering override deals with web category access requests, not DNS resolution manipulation.
- WildFire file blocking stops file transfers based on verdict, unrelated to DNS responses.
- Vulnerability Protection blocks exploit patterns in traffic, not DNS name resolution.

</details>

---

**Q6.** Which DNS Security subscription category specifically identifies domains generated algorithmically by malware to evade static blocklists (e.g., random strings like 'xk3f9q.com')?

A) DGA (Domain Generation Algorithm)
B) Command and Control (C2)
C) Grayware
D) Newly Registered Domains

<details>
<summary>Answer</summary>

**Correct: A**

The DGA category uses machine-learning-based domain analysis to flag algorithmically generated domain names, which malware families commonly use to rotate C2 infrastructure.

**Why the others are wrong:**
- C2 flags domains confirmed as active command-and-control infrastructure, not the algorithm used to generate the name itself.
- Grayware covers domains associated with adware/unwanted software, not algorithmic name generation.
- Newly Registered Domains flags recently created domains regardless of naming pattern, often used for phishing but distinct from DGA.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** Which log type shows the file hash, sample submission time, and the verdict returned by WildFire for a specific file?

A) Threat log
B) WildFire Submissions log
C) Data Filtering log
D) URL Filtering log

<details>
<summary>Answer</summary>

**Correct: B**

The WildFire Submissions log (Monitor > Logs > WildFire Submissions) records each file forwarded for analysis, along with its hash, file type, and returned verdict.

**Why the others are wrong:**
- The Threat log records the enforcement action taken once a signature exists (e.g., a matched malware signature blocking a session), not the original sandbox submission details.
- Data Filtering log tracks data pattern matches (e.g., DLP-style content matching), unrelated to WildFire verdicts.
- URL Filtering log records web category and action taken on URL requests, not file analysis verdicts.

</details>

---

**Q8.** A file submitted to WildFire returns a 'grayware' verdict. What does this verdict indicate about the file?

A) The file is confirmed malicious and actively distributing malware
B) The file is not malicious but exhibits unwanted or intrusive behavior, such as adware
C) The file could not be analyzed due to unsupported file type
D) The file is used specifically for credential phishing

<details>
<summary>Answer</summary>

**Correct: B**

Grayware is a distinct WildFire verdict for files that are not classified as malicious but display undesired behavior (adware, spyware-lite behaviors, nuisance software), separate from benign, malware, and phishing verdicts.

**Why the others are wrong:**
- Confirmed active malware distribution corresponds to the 'malware' verdict, not grayware.
- Unsupported file types are simply not forwarded/analyzed — this doesn't return a 'grayware' verdict at all.
- Credential-phishing-specific files return the dedicated 'phishing' verdict, introduced to separately flag phishing-related samples.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

