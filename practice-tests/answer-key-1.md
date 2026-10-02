# Answer Key — Practice Test 1

**Q1. Correct: D**

Virtual wire binds two interfaces together so the firewall passes traffic transparently at Layer 1/2 without requiring any IP addressing or routing changes, while still allowing full App-ID, Content-ID, User-ID, and threat prevention inspection inline.

---

**Q2. Correct: C**

Tap interfaces connect to a switch SPAN/mirror port and provide passive, out-of-band visibility into traffic for App-ID and threat detection reporting only — the firewall cannot block, drop, or modify traffic in this mode.

---

**Q3. Correct: D**

In the single-pass architecture, the packet is parsed and classified a single time, and all functions — networking, policy lookup, App-ID, Content-ID, threat/URL/DLP scanning — operate on that same classification within one flow, dramatically reducing overhead compared to UTM designs that re-scan traffic separately per feature.

---

**Q4. Correct: C**

A zone's type must correspond to the interface type of the members it contains (Tap, Virtual Wire, Layer 2, Layer 3, or Tunnel). Assigning a Tap interface to a Layer3-type zone causes a validation error and the configuration will fail commit.

---

**Q5. Correct: C**

Layer 2 mode allows the firewall to act as a transparent bridge for one or more VLANs. Configuring Layer 2 VLAN sub-interfaces lets each VLAN be mapped to its own zone for separate policy enforcement, all while preserving the existing Layer 2 broadcast domain and host IP addressing.

---

**Q6. Correct: B**

PAN-OS performs an initial policy lookup based on zone, source/destination IP, and port (application shown as unknown-tcp/udp) to allow the session to proceed for identification. Once enough packets allow App-ID to determine the actual application, PAN-OS re-checks the security policy against that identified application, and the session may be reclassified or denied if a more specific rule now applies.

---

**Q7. Correct: D**

In PAN-OS packet flow, the firewall performs the NAT policy lookup before the Security policy lookup, so the zone used for the Security rule match is determined by the POST-NAT address (placing the destination in the Trust zone where the real server resides). However, the Security policy itself always matches against the original, PRE-NAT IP addresses as they appeared on the wire. This combination (post-NAT zone, pre-NAT IP) is exactly how the rule must be written for the traffic to match correctly.

---

**Q8. Correct: B**

Using a subordinate CA issued by the enterprise root CA means the firewall-generated certificates chain to a CA already trusted by managed endpoints (via GPO/AD), so no browser warnings appear and outbound SSL Forward Proxy decryption works transparently.

---

**Q9. Correct: C**

U-Turn NAT handles the case where internal clients request the public IP of an internal resource. The rule matches traffic with source zone = internal and original destination = public IP, then translates the destination to the server's private IP while keeping both zones internal, allowing correct routing back through the firewall.

---

**Q10. Correct: C**

A Custom Application with a signature (context + pattern) allows App-ID to accurately classify the traffic while the session continues through the full App-ID/Content-ID pipeline, preserving threat prevention, URL filtering, and other Content-ID inspection.

---

**Q11. Correct: A**

PAN-OS Security policy is evaluated top-down; the first rule whose zone, address, application, user, and service criteria match the session is enforced, and evaluation stops at that point — this is why rule order matters critically for correct enforcement.

---

**Q12. Correct: C**

The Terminal Server Agent runs on the RDS/Citrix host itself and allocates a unique source port range per user session, allowing the firewall to disambiguate multiple users sharing the same source IP and map each session to the correct username.

---

**Q13. Correct: B**

SSL Inbound Inspection requires importing the actual server certificate and its private key onto the firewall, since the firewall must act as the endpoint of the TLS session to decrypt traffic destined to a server it does not sit in front of transparently as a proxy — this is only possible because the organization owns and controls the server's certificate.

---

**Q14. Correct: D**

Using a subordinate CA certificate issued by the internal enterprise PKI as the Forward Trust certificate lets the firewall dynamically generate per-site certificates that chain up to a root CA already trusted domain-wide via existing Group Policy, avoiding warnings without touching every endpoint individually.

---

**Q15. Correct: D**

The Terminal Server (TS) agent installs on the terminal/Citrix server itself and allocates a unique range of source ports to each user login session, allowing the firewall to distinguish individual users even though they all share the server's single IP address.

---

**Q16. Correct: C**

Security policy rules always match on the original (pre-NAT) destination IP address, so the rule must reference the public address 203.0.113.10; however, the destination zone is determined by a route lookup on the translated (post-NAT) address, which places the destination zone as DMZ where the real server resides.

---

**Q17. Correct: A**

WildFire uploads unknown files to a cloud (or WF-500 appliance) sandbox, executes them in a virtual environment, and returns a verdict; new signatures generated from the analysis are then distributed globally.

---

**Q18. Correct: B**

DNS Security provides cloud-delivered, machine-learning-based analysis that dynamically categorizes malicious domains, including those generated via DGA algorithms and DNS tunneling attempts.

---

**Q19. Correct: C**

The predefined strict profile sets critical, high, and medium severity vulnerability signatures to reset-both, immediately terminating the connection by sending resets to both the client and server.

---

**Q20. Correct: A**

DNS Sinkhole, configured under the DNS Policies section of an Anti-Spyware profile, forces resolution of malicious domains to a sinkhole IP address, letting the firewall and logs show which internal hosts attempted to reach malicious domains, simplifying identification of infected endpoints.

---

**Q21. Correct: D**

The Antivirus profile applies signature-based inline blocking of known malware, using signatures that include ones generated from prior WildFire verdicts, while WildFire itself performs dynamic sandbox analysis of unknown files to produce new signatures and verdicts.

---

**Q22. Correct: B**

The URL Filtering profile's User Credential Detection settings let administrators define, per URL category, whether valid corporate credential submissions are alerted, blocked, or allowed — the recommended control for preventing credential phishing to risky or uncategorized sites while permitting submission to trusted categories.

---

**Q23. Correct: D**

The 'sinkhole' action causes the firewall to forge a DNS response for malicious domains, returning the IP address of a sinkhole (either the Palo Alto Networks default sinkhole or a custom internal sinkhole IP). Infected hosts attempting to reach the malicious domain instead connect to the sinkhole IP, which shows up in traffic logs, letting administrators pinpoint the compromised internal host by its source IP.

---

**Q24. Correct: A**

In a Panorama template stack, the order of the templates determines precedence for overlapping configuration. The template positioned highest (closest to the top) in the stack list wins conflicts, so 'Base-Network' overrides 'Site-London' for the duplicated NTP setting.

---

**Q25. Correct: C**

Panorama's rule evaluation order for pre-rulebases is Shared first, then the device group hierarchy from the topmost parent down to the most specific child device group. So Shared runs first, then 'Global' (parent), then 'Branch-FW-01' (child), followed later by local firewall rules and then post-rules in reverse order.

---

**Q26. Correct: B**

For large-scale deployments, Palo Alto Networks recommends separating the management function from log collection by deploying dedicated Log Collectors (M-Series or Panorama in Log Collector mode) arranged into Collector Groups. This distributes log storage and processing load away from the Panorama management server.

---

**Q27. Correct: A**

Templates (and template stacks for combining multiple templates per firewall) are the Panorama construct used to push network and device configuration — interfaces, zones, NTP, DNS, and similar settings. Device groups are used separately for policies and objects, which matches the administrator's requirement to keep the two concerns apart.

---

**Q28. Correct: A**

Commit to Panorama only saves the candidate configuration into Panorama's running configuration; it does not send anything to managed devices. A separate Push to Devices operation, scoped to the appropriate device groups, templates, or collector groups, is required to deliver the configuration to firewalls.

---

**Q29. Correct: A**

Objects defined within a device group are inherited down the device group hierarchy, so they are visible to that device group and all of its descendant (child) device groups. Firewalls in 'DC-Firewalls' and its child 'DC-Firewalls-Branch' can therefore reference 'WebServer-DC1' in pushed policy.

---

**Q30. Correct: A**

In a template stack, template order defines priority, and the template positioned highest (topmost) in the stack takes precedence for any setting that is configured in more than one template. Non-overlapping settings from all templates are combined, but overlapping settings always resolve to the topmost template's value.

---

**Q31. Correct: B**

PAN-OS requires sequential download/install of the base image for each intermediate feature release between the current and target versions before installing the final target release; skipping feature-release base images is not supported and will fail dependency checks.

---

**Q32. Correct: A**

In active/active HA, the HA3 link is a dedicated dataplane link used specifically to forward packets between peers so the non-owner firewall can hand off received packets to the actual session owner, which is required to handle asymmetric routing.

---

**Q33. Correct: C**

HIP Match logs record which HIP objects and HIP profiles matched a connecting GlobalProtect endpoint, including specific posture attributes such as antivirus status, making them the correct source for endpoint compliance reporting.

---

**Q34. Correct: C**

Endpoints only trust the portal/gateway certificate if the issuing CA's root certificate is present in their trusted root store; distributing the enterprise root CA (e.g., via GPO) is required to avoid trust warnings.

---

**Q35. Correct: B**

A named configuration snapshot captures the configuration file itself, whereas exporting device state bundles the configuration plus additional runtime items (such as certificates and other device-specific data) needed to fully restore a replacement device during an RMA.

---

**Q36. Correct: B**

Preemption controls whether a higher-priority firewall automatically reclaims the active role once it becomes healthy again; disabling preemption on both peers ensures the currently active firewall stays active even after the original device's path issue is resolved.

---

**Q37. Correct: A**

PAN-OS upgrades must pass sequentially through each major release's base image (x.x.0) with a reboot after each step, and content updates should be current before starting the upgrade chain. Skipping a major version's base image is not supported and can cause the upgrade to fail or leave the device in an inconsistent state.

---

**Q38. Correct: D**

Binding gateway access to a client certificate profile ensures that only endpoints possessing the corporate-issued machine certificate can complete the TLS handshake and authenticate, effectively blocking personal devices regardless of whether they have valid user credentials.

---

**Q39. Correct: B**

In active/active HA, a floating IP address can move between the two firewalls based on device priority, allowing the NAT source address to remain consistent regardless of which unit processes the session, which avoids breaking upstream stateful connections during failover.

---

**Q40. Correct: D**

PAN-OS performs an initial policy lookup based on port/protocol, then re-evaluates the session once App-ID completes identification. If the identified application matches a more specific rule with a deny action, the session is torn down with 'policy-deny' even though an earlier port-based rule appeared to allow it. This App-ID re-match behavior is a classic PCNSE troubleshooting scenario.

---

**Q41. Correct: D**

'show counter global filter delta yes' displays incrementing global packet/session counters (e.g., flow_policy_deny, flow_fwd_l3_noroute) since the last invocation, letting the administrator pinpoint the exact processing stage responsible for drops without running a full packet capture.

---

**Q42. Correct: D**

In SSL Forward Proxy, the firewall re-signs the server certificate with its own Forward Trust CA. If that CA is not installed as a trusted root on client devices, browsers correctly flag the re-signed certificate as untrusted. Distributing the CA via GPO/MDM or manual install resolves the warnings.

---

**Q43. Correct: A**

The GlobalProtect gateway presents its SSL/TLS service profile certificate during tunnel establishment. If that certificate is self-signed or issued by a CA not trusted by the client's operating system/certificate store, the client rejects the connection with a certificate trust error, even though portal authentication (a separate connection) succeeded.

---

**Q44. Correct: B**

The HA1 control link carries heartbeats and state synchronization messages used to determine peer health. Without a redundant HA1 backup link, a single failure of the sole control link causes both members to lose contact with each other and independently assume the active role, producing split-brain. Best practice is to always configure an HA1 backup link.

---

**Q45. Correct: A**

PAN-OS allows administrators to place a commit lock (and/or config lock) on the candidate configuration to prevent other users from committing conflicting changes simultaneously. The lock must be manually released by the owning administrator or a superuser before another commit can succeed.

---

**Q46. Correct: B**

Phase 1 (IKE) establishing successfully but Phase 2 (IPSec SA) failing with 'proposal not accepted' points directly to a mismatch in the Phase 2 negotiation parameters — either the proxy-ids (the local/remote traffic selectors that define which subnets are protected by the tunnel) or the IPSec Crypto profile settings (encryption algorithm, authentication algorithm, DH group, or lifetime) not matching between the two peers. This is the classic PCNSE scenario for a tunnel that negotiates IKE but never brings up the actual data tunnel.

---

