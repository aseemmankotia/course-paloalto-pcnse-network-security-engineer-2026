## Chapter 5: Threat Prevention: Antivirus, Anti-Spyware, and Vulnerability Protection — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | What scanning method does the Antivirus profile use? | Stream-based scanning — inspects file content as it passes through the firewall without buffering the whole file. Exam tip: contrast with WildFire, which requires full file capture for sandbox analysis. |
| 2 | Default Antivirus profile action for SMTP/IMAP/POP3 decoders vs HTTP/FTP/SMB? | Email decoders (smtp, imap, pop3) default to alert; http, ftp, and smb default to block. Exam gotcha: the default AV profile does NOT block email-borne malware by default. |
| 3 | Where is DNS Sinkholing configured? | Inside the Anti-Spyware profile, under the DNS Signatures/DNS Policies section, by setting the action to 'sinkhole' and specifying a sinkhole IP. |
| 4 | Why use DNS sinkholing instead of simply blocking a malicious DNS query? | It redirects the malicious domain's resolution to a sinkhole IP so the infected internal host's follow-up connection attempt appears in traffic logs — revealing the true infected client even when an internal DNS server masks it. |
| 5 | What severity levels exist in Anti-Spyware and Vulnerability Protection profile rules? | Critical, High, Medium, Low, Informational — each can be assigned an independent action. |
| 6 | List the possible actions for Anti-Spyware / Vulnerability Protection profile rules. | default, allow, alert, drop, reset-client, reset-server, reset-both, block-ip. |
| 7 | What profiles can a Security Profile Group bundle together? | Antivirus, Anti-Spyware, Vulnerability Protection, URL Filtering, File Blocking, and WildFire Analysis (plus Data Filtering) into one object attached to a security policy rule. |
| 8 | True or False: The default Vulnerability Protection profile blocks all critical-severity exploits. | False — many default rules only alert. Use the predefined 'strict' profile or manually set critical/high severity actions to reset-both/drop for stronger enforcement. Major exam gotcha. |
| 9 | How do you tune a false-positive threat signature without disabling the entire profile? | Add an exception for the specific signature ID under the profile's Exceptions tab; optionally scope it to specific source/destination IP addresses. |
| 10 | What does the Vulnerability Protection profile primarily detect? | Exploit attempts against known software/OS vulnerabilities using signature matching (e.g., buffer overflows, remote code execution attempts). |
| 11 | Do security profiles apply to 'deny' security policy rules? | No — security profiles (AV, Anti-Spyware, Vulnerability Protection, URL Filtering, File Blocking, WildFire) only take effect on rules where the action is 'allow'. |
| 12 | What additional element must be configured for the sinkhole action to work? | A Sinkhole IPv4/IPv6 address, configured in the DNS Signature Policy settings. Palo Alto Networks provides a default sinkhole FQDN (sinkhole.paloaltonetworks.com) for this purpose. |
