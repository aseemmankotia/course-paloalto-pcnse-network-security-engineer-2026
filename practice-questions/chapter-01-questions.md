## Chapter 1: Single-Pass Architecture and Packet Flow Fundamentals — Practice Questions

**Q1.** A company wants to evaluate a PA-Series firewall on a production network by connecting it to a switch SPAN/mirror port. They want visibility into application usage without any risk of impacting existing traffic. Which deployment mode should they use?

A) Layer 3
B) Virtual wire
C) Tap mode
D) Layer 2

<details>
<summary>Answer</summary>

**Correct: C**

Tap mode connects to a mirror/SPAN port and only receives a copy of traffic for visibility — it cannot alter or block traffic, making it zero-risk for a production PoC.

**Why the others are wrong:**
- Layer 3 requires readdressing and routing changes, and is inline — too risky for a passive PoC.
- Virtual wire is inline and can drop traffic, which is more risk than the scenario requires.
- Layer 2 requires the firewall to switch traffic inline between VLANs, not passively monitor it.

</details>

---

**Q2.** A network team must insert a firewall between a core router and a switch without changing any existing IP addressing or VLAN design, and they need the firewall to actively block malicious traffic immediately after deployment. Which deployment mode BEST meets these requirements?

A) Tap mode
B) Layer 3 with static NAT
C) Layer 2 with new VLAN objects
D) Virtual wire

<details>
<summary>Answer</summary>

**Correct: D**

Virtual wire binds two interfaces together and passes traffic transparently with no MAC/IP changes, while still being fully inline and able to enforce security policy.

**Why the others are wrong:**
- Tap mode is passive only and cannot block traffic.
- Layer 3 requires new IP addressing on the firewall interfaces, violating the 'no changes' requirement.
- Layer 2 requires creating new VLAN objects and reconfiguring switching, which is more change than needed.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** An administrator moves an interface from zone 'Trust' into a newly created zone 'Trust-DMZ'. The security rule 'Trust-to-Untrust' still exists unchanged and previously allowed all traffic from Trust. After the move, users on that interface can no longer reach the internet. What is the most likely cause?

A) Security policy matches on zone, not IP address, so traffic no longer matches any rule referencing the new zone
B) The session table was not cleared after the zone change
C) The virtual router lost its default route after the zone change
D) App-ID cached the old zone and requires a reboot to update

<details>
<summary>Answer</summary>

**Correct: A**

Security policies evaluate source/destination ZONE, not IP address. Moving an interface to a new zone means traffic no longer matches rules written for the old zone, even though IP addressing is unchanged.

**Why the others are wrong:**
- Stale sessions could cause temporary issues but wouldn't explain persistent failure to reach a rule match — new sessions would still fail policy lookup.
- Zone changes don't affect virtual router routing tables directly.
- There is no zone caching in App-ID that requires a reboot; zone lookup happens per-session at policy evaluation.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** An administrator needs to insert a firewall transparently between a switch trunk carrying VLAN 10 and VLAN 20, apply different security policies to each VLAN, and avoid reconfiguring the switch's Layer 3 setup. Which feature should be used?

A) Layer 2 subinterfaces bound to VLAN objects
B) Tap mode with two separate zones
C) Layer 3 subinterfaces bound to a virtual router with VLAN trunk
D) Virtual wire subinterfaces tagged with VLAN IDs, each assigned to a different zone

<details>
<summary>Answer</summary>

**Correct: D**

Virtual wire subinterfaces can match specific VLAN tags and be assigned to distinct zones, allowing per-VLAN policy while remaining fully transparent — no L3 or switching changes needed.

**Why the others are wrong:**
- Layer 2 subinterfaces require the firewall to act as a switch, which changes the L2 forwarding design more than a vwire insertion.
- Tap mode cannot enforce differentiated security policy — it's visibility only.
- Layer 3 requires IP addressing and routing changes, violating the 'no L3 reconfiguration' requirement.

</details>

---

**Q5.** A firewall interface must route traffic between two subnets and participate in OSPF. Which object must this interface be assigned to in order to perform routing?

A) A zone only
B) A virtual router
C) A virtual wire
D) A VLAN object

<details>
<summary>Answer</summary>

**Correct: B**

Routing (static routes, OSPF, BGP, RIP, ECMP) is configured and performed at the virtual router level in PAN-OS; the interface must be assigned to a virtual router to route traffic.

**Why the others are wrong:**
- Zones control policy lookup, not routing — a zone alone does not enable routing.
- Virtual wire interfaces don't route; they pass traffic transparently at Layer 1/2.
- VLAN objects are used in Layer 2 deployments for switching, not routing.

</details>

---

**Q6.** Which sequence correctly represents PAN-OS packet flow for the first packet of a brand-new TCP session?

A) Session lookup → Zone lookup → Policy lookup → App-ID → Content-ID → Forwarding
B) App-ID → Content-ID → Zone lookup → Policy lookup → Forwarding
C) Policy lookup → Zone lookup → Session lookup → Forwarding → App-ID
D) Zone lookup → Session lookup (miss) → Policy lookup → App-ID → Content-ID → Forwarding → Egress

<details>
<summary>Answer</summary>

**Correct: D**

PAN-OS parses the packet, performs zone lookup for the ingress interface, checks the session table (a miss triggers slow-path processing), performs an initial policy lookup, runs App-ID, then Content-ID, then forwards the packet out the egress interface.

**Why the others are wrong:**
- Session lookup must occur after zone lookup — the zone is needed as part of the session key/policy match, not before it's determined.
- App-ID and Content-ID cannot run before a zone and policy lookup — the firewall must first determine if traffic is even permitted to reach app inspection.
- Policy lookup requires the zone to already be known, so zone lookup cannot happen after policy lookup.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** What is the PAN-OS default session timeout value for UDP sessions?

A) 3600 seconds
B) 60 seconds
C) 30 seconds
D) 6 seconds

<details>
<summary>Answer</summary>

**Correct: C**

The default UDP session timeout in PAN-OS is 30 seconds, reflecting UDP's connectionless nature.

**Why the others are wrong:**
- 3600 seconds is the default TCP session timeout, not UDP.
- 60 seconds is not a standard PAN-OS default timeout value.
- 6 seconds is the default ICMP timeout, not UDP.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** What is the primary advantage of PAN-OS single-pass architecture compared to traditional multi-engine, multi-pass firewalls?

A) It uses more CPU cores to run each security engine as a separate scanning pass
B) It eliminates the need for signature database updates
C) It guarantees unlimited concurrent sessions regardless of hardware
D) The packet is parsed and classified once, and App-ID, Content-ID, and User-ID all operate on that same parsed data, reducing latency

<details>
<summary>Answer</summary>

**Correct: D**

Single-pass software means the packet is parsed and classified one time; all security functions act on that single classification, avoiding redundant re-parsing that multi-pass architectures require.

**Why the others are wrong:**
- This describes multi-pass, multi-engine designs — the opposite of single-pass architecture.
- Single-pass architecture has nothing to do with signature update frequency.
- Session capacity is determined by hardware/platform specs, not by the single-pass design itself.

</details>

---

