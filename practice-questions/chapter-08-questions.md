## Chapter 8: PAN-OS Upgrades, High Availability, and Configuration Management — Practice Questions

**Q1.** A firewall is running PAN-OS 10.1.9 and must be upgraded to PAN-OS 11.1.2. Which upgrade path is correct?

A) Download and install the 11.1.2 image directly onto the 10.1.9 firewall
B) Downgrade to PAN-OS 10.0 first, then upgrade straight to 11.1.2
C) Install only the 11.0.3 image, skipping 10.2 entirely, then move to 11.1.2
D) Install the 10.2 base image, then the 11.0 base image, then upgrade to 11.1.2

<details>
<summary>Answer</summary>

**Correct: D**

PAN-OS requires sequential installation of each major release's base image (10.2, then 11.0) before installing the target major/maintenance release — you cannot skip a major version.

**Why the others are wrong:**
- Direct install fails preflight checks; the firewall blocks jumps across more than one major release.
- Downgrading is unnecessary and doesn't satisfy the sequential base-image requirement.
- Skipping the 10.2 base image is exactly the mistake the exam tests — major releases cannot be skipped.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** In an active/passive HA pair, the active firewall fails and the passive firewall takes over. The failed firewall recovers and rejoins the HA pair. Preemption is disabled on both firewalls. What happens?

A) The recovered firewall automatically reclaims the active role because it has a lower (higher-priority) priority value
B) Both firewalls become active simultaneously, causing a split-brain condition
C) The recovered firewall becomes passive and stays passive; the current active firewall remains active
D) The recovered firewall forces a re-election every 60 seconds until it becomes active again

<details>
<summary>Answer</summary>

**Correct: C**

Without preemption enabled, a firewall that regains health does not force an active-role takeover; it rejoins as passive and the current active device stays active.

**Why the others are wrong:**
- Automatic reclaiming only happens if preemption is enabled on both devices — it is disabled here.
- Active/passive is designed to prevent both firewalls being active at once; split-brain is not the expected behavior.
- There is no forced periodic re-election loop in HA — state changes are event-driven (link/monitor failures), not timer-driven reclaims.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** An active/active HA deployment experiences asymmetric routing where the return traffic arrives at the firewall that did not process the initial session setup. Which HA link is responsible for forwarding that packet to the correct firewall?

A) HA3 (packet forwarding link)
B) HA1 (control link)
C) HA2 (data link)
D) HA1 backup link

<details>
<summary>Answer</summary>

**Correct: A**

HA3 exists only in active/active mode and forwards packets between HA peers when asymmetric traffic arrives at the non-session-owner firewall.

**Why the others are wrong:**
- HA1 only carries heartbeats, hellos, and configuration/state sync messages — no packet forwarding.
- HA2 synchronizes session state and forwarding tables between peers but does not carry the live asymmetric packet itself in active/active mode.
- The HA1 backup link is a redundant control path for HA1, not a data-forwarding link.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** Before making a significant configuration change to a production firewall, an administrator wants a restorable backup of the current configuration. Which action should be performed?

A) Click Commit to push candidate changes to running configuration
B) Export a named configuration snapshot from Device > Setup > Operations
C) Run 'show config running' and copy the CLI output manually
D) Enable configuration auto-sync to the HA peer

<details>
<summary>Answer</summary>

**Correct: B**

Exporting a named configuration snapshot (XML) via Device > Setup > Operations creates a downloadable, restorable backup of the current running configuration before changes are made.

**Why the others are wrong:**
- Commit applies candidate changes to running config — it does not create a separate backup file.
- Manually copying CLI output is not a supported, restorable backup format for PAN-OS.
- HA config sync keeps peers consistent but is not a backup mechanism and doesn't protect against a bad change being pushed to both devices.

</details>

---

**Q5.** An administrator makes changes in the Web UI but has not clicked Commit. Which configuration file currently contains those unsaved changes?

A) running-config.xml
B) backup-config.xml
C) candidate-config.xml
D) startup-config.xml

<details>
<summary>Answer</summary>

**Correct: C**

All uncommitted edits are stored in candidate-config.xml until a Commit operation copies them into running-config.xml.

**Why the others are wrong:**
- running-config.xml holds only the last committed, active configuration — not pending edits.
- PAN-OS does not use a file named backup-config.xml as part of the candidate/running workflow.
- startup-config.xml is not a PAN-OS configuration file naming convention (that terminology belongs to other vendors).

</details>

---

**Q6.** Palo Alto Networks best practice recommends configuring which threshold behavior for Applications and Threats content updates in a production environment?

A) Install updates immediately upon download with no delay
B) Disable all content updates and apply them manually once per year
C) Only download and install WildFire signatures, skip Applications and Threats entirely
D) Download updates immediately but delay installation by a threshold (commonly 1 hour or more) to avoid false-positive impact

<details>
<summary>Answer</summary>

**Correct: D**

Best practice is to download App/Threat content immediately but set an install threshold delay so newly released signatures have time to stabilize before enforcement, reducing false-positive risk.

**Why the others are wrong:**
- Zero-delay install increases risk of a bad or overly aggressive signature immediately blocking legitimate traffic.
- Annual manual updates leave the firewall exposed to new threats for far too long and fail the exam's 'stay current' expectation.
- Skipping App and Threat updates entirely disables core IPS/App-ID functionality, which the exam explicitly tests as incorrect practice.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** Two firewalls in an HA pair are configured with device priority 100 and 200 respectively, and preemption is enabled. Which firewall becomes active by default?

A) The firewall with priority 200, because higher numbers indicate higher priority
B) Whichever firewall boots up first, regardless of priority value
C) The firewall with priority 100, because a lower numeric value indicates higher priority
D) Both firewalls, since priority only matters in active/active mode

<details>
<summary>Answer</summary>

**Correct: C**

In PAN-OS HA, a lower numeric priority value means higher priority; the firewall with priority 100 wins the election and becomes active.

**Why the others are wrong:**
- This reverses the actual rule — higher numbers mean lower priority, not higher.
- Boot order can influence which device becomes active first, but once both are up, priority (with preemption enabled) determines the final active device.
- Priority values are used in both active/passive and active/active elections, not exclusively active/active.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** In an active/active HA pair, what is the functional difference between the 'session owner' and 'session setup' roles?

A) Session owner is always the firewall with the lower HA priority; session setup is always the other peer
B) Session owner handles only Layer 2 traffic; session setup handles only Layer 3 traffic
C) There is no distinction — both firewalls own and set up every session identically in active/active mode
D) Session setup is the firewall that receives the first packet and creates the session; session owner is the firewall responsible for that session's aging, logging, and policy re-evaluation

<details>
<summary>Answer</summary>

**Correct: D**

Session setup is determined by which firewall first processes the initial packet; session owner is the firewall assigned to manage that session's state, aging, and logging for its duration, which may or may not be the same device.

**Why the others are wrong:**
- Priority determines HA election/active state, not which device owns or sets up an individual session.
- The session owner/setup distinction has nothing to do with Layer 2 vs Layer 3 traffic types.
- Active/active explicitly separates these two roles per session — they are not identical or interchangeable.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

