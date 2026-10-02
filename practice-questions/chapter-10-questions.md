## Chapter 10: Structured Troubleshooting: Packet Flow, Logs, and Packet Captures — Practice Questions

**Q1.** A PCNSE candidate needs to verify which security rule will match a specific flow (source zone trust, source IP 10.1.1.5, destination zone untrust, destination IP 8.8.8.8, application dns, port 53) WITHOUT sending live traffic through the firewall. Which CLI command should be used?

A) test nat-policy-match from trust to untrust destination 8.8.8.8
B) test security-policy-match source 10.1.1.5 destination 8.8.8.8 destination-port 53 protocol 17 from trust to untrust
C) show session all filter destination 8.8.8.8
D) debug dataplane packet-diag set capture on

<details>
<summary>Answer</summary>

**Correct: B**

test security-policy-match simulates a flow against the running security policy set and returns the exact rule that would be matched, including implicit rules, without requiring live traffic.

**Why the others are wrong:**
- test nat-policy-match evaluates NAT rules, not security policy rule matching, and won't confirm which security rule applies.
- show session all filter only displays sessions already established in the session table — it cannot predict a match for traffic that hasn't been sent.
- debug dataplane packet-diag set capture on configures packet capture stages; it captures real packets, it does not simulate a policy match.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A traffic log entry shows session end reason 'decrypt-cert-validation' for an SSL Forward Proxy session. What is the MOST likely root cause?

A) The client does not trust the firewall's forward-trust CA certificate
B) The negotiated cipher suite is not supported by the firewall's hardware
C) The Decryption policy rule has no matching security rule below it
D) The server certificate failed validation (expired, untrusted CA, or revoked) and the Decryption Profile is set to block on cert errors

<details>
<summary>Answer</summary>

**Correct: D**

'decrypt-cert-validation' as a session end reason specifically indicates the firewall detected a problem validating the destination server's certificate (expiry, untrusted issuer, or revocation) and the applicable Decryption Profile is configured to block the session on that failure.

**Why the others are wrong:**
- Client mistrust of the forward-trust CA produces browser-side certificate warnings, not a firewall-logged decrypt-cert-validation session end reason.
- Unsupported cipher suites are logged with a distinct reason such as 'decrypt-unsupport-param' or shown in the Decryption log as an unsupported algorithm, not cert validation.
- Missing an underlying security rule would result in a policy-deny action on the security log, not a decrypt-cert-validation end reason.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** Two firewalls in an active/passive HA pair both transition to the active state simultaneously, and each begins forwarding traffic independently. HA2 (data link) shows up, but HA1 (control link) shows down. What condition is occurring, and what should be checked FIRST?

A) HA3 packet-forwarding link failure — check the HA3 interface cabling
B) Split-brain caused by HA1 control link failure — check HA1 link connectivity and heartbeat/hello configuration
C) Normal active/active behavior — no action required
D) Session synchronization failure — check HA2 keep-alive threshold

<details>
<summary>Answer</summary>

**Correct: B**

The HA1 control link carries heartbeats and state information used to negotiate which peer is active. If HA1 fails while HA2 (data) remains up, both peers can independently decide they are active — this is the classic split-brain scenario. The first troubleshooting step is verifying HA1 physical connectivity, IP config, and heartbeat settings.

**Why the others are wrong:**
- HA3 is used only in active/active mode for packet forwarding of asymmetric flows; it is unrelated to a control-plane split-brain condition in active/passive.
- Two firewalls simultaneously active in an active/passive pair is never normal behavior — it indicates a failure state.
- HA2 keep-alive governs session/config sync between peers, but since HA2 is reported up in this scenario, it is not the first thing to check.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** An administrator suspects that outbound traffic is being translated to the wrong public IP address. Which CLI command directly confirms which NAT rule and translated address will be applied to a specific flow?

A) test security-policy-match source 10.1.1.5 destination 8.8.8.8
B) show running nat-policy
C) test nat-policy-match from trust to untrust source 10.1.1.5 destination 8.8.8.8 destination-port 443 protocol 6
D) debug dataplane pool-stats

<details>
<summary>Answer</summary>

**Correct: C**

test nat-policy-match evaluates the NAT rulebase against a specified flow and returns the exact NAT rule and translated source/destination that would be applied — the direct tool for confirming translation behavior.

**Why the others are wrong:**
- test security-policy-match evaluates only the security rulebase, not NAT translation.
- show running nat-policy displays the configured NAT rules in order but does not simulate which rule matches a specific flow.
- debug dataplane pool-stats is not a valid troubleshooting command for NAT rule matching; it is unrelated to NAT policy evaluation.

</details>

---

**Q5.** During a packet capture troubleshooting session, an administrator wants to see packets AFTER they have passed security policy processing but BEFORE they leave the egress interface. Which packet capture stage should be enabled?

A) firewall
B) receive
C) transmit
D) drop

<details>
<summary>Answer</summary>

**Correct: A**

The 'firewall' stage captures packets after they have completed security processing (policy lookup, NAT, content inspection) but before they are queued for transmission out the egress interface.

**Why the others are wrong:**
- receive captures packets as they arrive on the ingress interface, before any policy processing occurs.
- transmit captures packets as they actually leave the egress interface, after the firewall stage.
- drop captures packets that were discarded at any processing point, not packets successfully passing through.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** A company needs to confirm, for a live session already established on the firewall, exactly which security rule, NAT rule, and application were matched — without generating any new test traffic. Which command BEST satisfies this requirement?

A) test security-policy-match with the session's 5-tuple
B) debug dataplane packet-diag set capture on
C) show session id <id>
D) show counter global filter delta yes

<details>
<summary>Answer</summary>

**Correct: C**

show session id <id> displays complete details of an existing live session, including the matched security rule, NAT rule, application, zones, and session flags — exactly what's needed to audit an already-established flow.

**Why the others are wrong:**
- test security-policy-match simulates a NEW flow against policy; it does not inspect an existing session's actual matched rule and state.
- debug dataplane packet-diag set capture on enables packet capture for new packets; it does not report details of an already-established session.
- show counter global provides dataplane-wide statistics/counters and is not session-specific.

</details>

---

**Q7.** In a traffic log, which session end reason value explicitly indicates the session was terminated because a security policy rule denied the traffic (as opposed to a TCP reset or timeout)?

A) tcp-rst-from-client
B) policy-deny
C) aged-out
D) tcp-fin

<details>
<summary>Answer</summary>

**Correct: B**

policy-deny is logged when a security policy rule with a deny/drop action explicitly terminates the session — it directly points to a policy-match issue rather than a network-layer event.

**Why the others are wrong:**
- tcp-rst-from-client indicates the client sent a TCP RST, an endpoint-driven termination unrelated to policy action.
- aged-out means the session exceeded its idle timeout with no policy denial involved.
- tcp-fin indicates a normal graceful TCP closure initiated by an endpoint, not a policy action.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** An administrator wants to determine, before deploying a URL filtering policy, which category a specific URL belongs to according to PAN-DB. Which CLI command should be used?

A) test security-policy-match application web-browsing
B) show url-cache all
C) test nat-policy-match destination <url>
D) test url-info-cloud <url>

<details>
<summary>Answer</summary>

**Correct: D**

test url-info-cloud <url> (or test url-info-host for the local cache) queries PAN-DB directly to return the URL category, confirming how URL Filtering policy will classify it before deployment.

**Why the others are wrong:**
- test security-policy-match only evaluates security rules by application/zone/IP — it cannot look up URL categories.
- show url-cache all displays cached local URL category entries but does not perform a live lookup or confirm cloud categorization for a specific unseen URL.
- test nat-policy-match requires IP-based matching criteria, not a URL string, and is unrelated to URL categorization.

</details>

---

