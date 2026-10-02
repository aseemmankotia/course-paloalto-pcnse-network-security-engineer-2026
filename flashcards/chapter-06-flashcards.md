## Chapter 6: Advanced URL Filtering, WildFire, and DNS Security — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | URL Filtering action requiring end user to click through a warning page (no password)? | Continue. Logged as a click-through event. Exam trap: don't confuse with Alert, which logs silently with no interactive page. |
| 2 | URL Filtering action requiring an admin-defined password to bypass a block page? | Override — password set under Device > Setup > Content-ID > URL Admin Override. |
| 3 | Where are per-file-type WildFire maximum forwarding sizes configured? | Device > Setup > WildFire (General Settings tab) — separate from the WildFire Analysis security profile, which controls what gets forwarded, not how big. |
| 4 | Default WildFire forwarding size limit for PE (executable) files? | 10 MB by default, increasable up to the platform/cloud-supported maximum. Most heavily tested file-size value on the exam. |
| 5 | Default WildFire forwarding size limit for APK (Android) files? | 30 MB by default — higher than PE because Android packages are typically larger. |
| 6 | The four possible WildFire verdicts? | Benign, Grayware, Malware, Phishing. Grayware = unwanted but not malicious (adware-type behavior). |
| 7 | Core DNS Security subscription categories tested on PCNSE? | Malware, Phishing, Command-and-Control (C2), Grayware, DGA, Newly Registered Domains, Parked Domains — configured under Anti-Spyware profile DNS Policies. |
| 8 | Which log records the file hash and returned WildFire verdict for a submitted sample? | WildFire Submissions log (Monitor > Logs > WildFire Submissions) — distinct from the Threat log, which shows enforcement once a signature exists. |
| 9 | What DNS Security/Anti-Spyware action redirects a malicious domain's DNS response to a controlled IP for detection? | Sinkhole — lets the SOC trace which internal host is infected by watching who tries to connect to the sinkhole IP. |
| 10 | Local/private WildFire appliance (WF-500) vs public WildFire cloud — key exam distinction? | WF-500 keeps sample analysis on-prem for data residency/compliance; public cloud offers broader global threat intelligence and faster signature rollout. |
| 11 | How fast are new WildFire signatures distributed with an active WildFire subscription vs without one? | With subscription: as often as every 5 minutes. Without subscription: malware coverage relies on the much slower daily Antivirus content update instead. |
| 12 | URL Filtering action: Block vs Continue — exam distinction? | Block = full deny, session never proceeds. Continue = warning page shown, user clicks through, session then proceeds and is logged. |
