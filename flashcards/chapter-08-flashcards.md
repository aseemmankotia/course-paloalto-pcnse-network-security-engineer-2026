## Chapter 8: PAN-OS Upgrades, High Availability, and Configuration Management — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | PAN-OS major release upgrade rule? | Must install each intervening major release's base image sequentially — cannot skip a major version. Exam tip: e.g. 10.1 → must install 10.2 base → 11.0 base → then target 11.1. |
| 2 | HA1 link purpose and traffic type? | Control link: carries heartbeats, hellos, HA state info, and configuration/routing sync between peers. Exam tip: does NOT carry session or packet data. |
| 3 | HA2 link purpose? | Data link: synchronizes session table, forwarding table, and IPsec SA info between HA peers. Exam tip: HA2 is unidirectional from active to passive in active/passive mode. |
| 4 | HA3 link — when is it used? | Only in active/active HA mode, for packet forwarding when asymmetric traffic arrives at the non-owning firewall. Exam tip: HA3 does not exist in active/passive deployments. |
| 5 | HA device priority: lower or higher number wins election? | Lower numeric value = higher priority = becomes active. Exam tip: reverse of what many students assume — memorize 'lower wins'. |
| 6 | Is preemption enabled by default in PAN-OS HA? | No — disabled by default. Must be manually enabled on BOTH firewalls to allow a recovered higher-priority device to reclaim active role. |
| 7 | Candidate config vs running config — what's the difference? | candidate-config.xml = uncommitted pending edits. running-config.xml = last committed, active configuration. Commit copies candidate → running. |
| 8 | How do you create a restorable firewall configuration backup? | Device > Setup > Operations > Export named configuration snapshot (downloads an XML backup). Exam tip: this is distinct from just clicking Commit. |
| 9 | Active/passive vs active/active HA — key operational difference? | Active/passive: only one firewall processes traffic at a time. Active/active: both firewalls process traffic concurrently, requiring session-owner/session-setup role assignment and HA3 for asymmetric flows. |
| 10 | Session owner vs session setup role (active/active)? | Session setup = firewall that received the first packet and created the session. Session owner = firewall responsible for aging, logging, and policy re-evaluation of that session over its lifetime. |
| 11 | Best practice for App/Threat content update installation timing? | Download immediately, but delay install with a threshold (commonly 1 hour+) to let new signatures stabilize and avoid false positives before enforcement. |
| 12 | Content update types tested on PCNSE? | Applications and Threats (App-ID/IPS signatures), Antivirus, and WildFire signatures — each has its own independent schedule/threshold settings in Device > Dynamic Updates. |
