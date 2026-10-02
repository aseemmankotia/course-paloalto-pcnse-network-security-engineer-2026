## Chapter 2: Security Policy, NAT, and Zone/Interface Design — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Security policy rule evaluation order in PAN-OS? | Top-down, first match wins — evaluation stops at the first rule that matches all criteria. Exam tip: broad rules above specific rules cause 'shadowing.' |
| 2 | What zone does security policy use for matching — pre-NAT or post-NAT? | Post-NAT zone, but pre-NAT IP address (except destination NAT, where the post-NAT destination IP must be used in the rule). |
| 3 | Destination NAT security policy exception — which IP goes in the rule? | Use the POST-NAT (translated) destination IP address in the security policy rule, not the original pre-NAT public IP. |
| 4 | What is U-turn (hairpin) NAT used for? | Allows internal clients to reach an internal server via its public NAT IP, by combining source NAT + destination NAT in one NAT rule; requires proper zone matching in security policy. |
| 5 | Dynamic IP and Port (PAT) vs Dynamic IP (no PAT) — key difference? | PAT translates many-to-one using port multiplexing (most common for outbound internet access). Dynamic IP (no PAT) is one-to-one from a pool without port translation — session drops if pool exhausted unless fallback configured. |
| 6 | Static NAT behavior? | Persistent, bi-directional one-to-one IP translation. Commonly used to publish an internal server with a fixed public IP. |
| 7 | Valid VLAN tag range for Layer 3 subinterfaces? | 1–4094 (802.1Q). Tag 0 and 4095 are reserved. Exam tip: subinterfaces let one physical port serve multiple zones/VRs. |
| 8 | What is an Aggregate Ethernet (AE) interface used for? | Bundles multiple physical interfaces into a single logical link for bandwidth aggregation and redundancy (LACP supported). Not used for zone segmentation like subinterfaces. |
| 9 | What is a Loopback interface commonly used for on PAN-OS? | Management access, VPN tunnel termination, or NAT translated addresses — has no physical link but must still be assigned a zone. |
| 10 | What does Policy-Based Forwarding (PBF) override? | The standard routing table (forwarding/route lookup) for traffic matching specific criteria (zone, address, app, user) — used to force traffic out a specific link/interface regardless of the route table. |
| 11 | Rule shadowing — definition and exam trap? | A broad rule placed ABOVE a specific rule matches first and prevents the specific rule from ever being evaluated. Exam commonly tests reordering rules to fix this. |
| 12 | NAT policy evaluation order relative to security policy? | NAT policy is evaluated first to determine the translation and resulting egress zone/interface; the security policy is then evaluated using pre-NAT addresses and the post-NAT zone. |
