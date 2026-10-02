# Chapter 1 Cheat Sheet: SP3 Architecture & Packet Flow

## Packet Flow Order (First Packet / New Session)
| Step | Action |
|---|---|
|1|Ingress interface receives packet|
|2|Zone lookup (based on ingress interface's zone binding)|
|3|Session lookup — miss triggers slow path|
|4|Initial policy lookup (zone, IP, port, port-based App-ID guess)|
|5|NAT policy lookup / session created|
|6|App-ID engine (may re-evaluate policy once app confirmed)|
|7|Content-ID (AV, anti-spyware, vuln protection, URL filtering, WildFire)|
|8|Forwarding lookup (virtual router / route table)|
|9|Egress interface, NAT applied, packet sent|

Subsequent packets matching an existing session use the **fast path**: skip full policy lookup, continue App-ID until app is fully identified, then Content-ID → forwarding.

## Deployment Mode Comparison
| Mode | Inline? | Can Block Traffic? | IP Addressing Needed? | Notes |
|---|---|---|---|---|
|Tap|No (SPAN/mirror)|No|No|Visibility only, zero risk|
|Virtual Wire|Yes|Yes|No|Transparent, supports VLAN-tagged subinterfaces|
|Layer 2|Yes|Yes|No (uses VLAN objects)|Firewall acts as switch, MAC learning|
|Layer 3|Yes|Yes|Yes|Requires virtual router, supports routing protocols|

## Zones
- Every interface is bound to exactly **one** zone at a time; multiple interfaces can share a zone.
- Security policy match = source zone + destination zone (not raw IPs).
- Default rules: **intrazone-default = allow**, **interzone-default = deny**.
- Changing an interface's zone can silently break existing rules written for the old zone.

## Virtual Routers
- Required for Layer 3 deployments.
- Hold the RIB; support static routes, OSPF, BGP, RIP, ECMP.
- Multiple VRs allow routing-table separation (e.g., per tenant/segment).

## Session Timeouts (Defaults)
| Protocol/State | Timeout |
|---|---|
|TCP (established)|3600s|
|TCP handshake|10s|
|TCP half-closed|120s|
|TCP time-wait|15s|
|UDP|30s|
|ICMP|6s|

## Useful CLI (Operational Mode)
```
show session all
show session id <id>
show session info
show interface all
show zone
show routing route
```

## Exam Gotchas
- Tap mode **cannot** enforce/block — visibility only.
- Vwire needs **no IP addressing**; Layer 3 does.
- Zone lookup happens **before** session lookup; session miss triggers full slow-path processing.
- Policy is matched on **zone**, not interface IP — reassigning zones without updating rules is a top real-world and exam trap.
- Single-pass = software (parse once); parallel processing = hardware (simultaneous engines) — don't conflate the two terms.
