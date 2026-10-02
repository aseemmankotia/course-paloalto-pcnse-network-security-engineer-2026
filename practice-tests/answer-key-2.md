# Answer Key — Practice Test 2

**Q1. Correct: B**

Tap mode is strictly passive/out-of-band — the firewall only receives a mirrored copy of traffic and has no ability to forward, block, or inject packets. Security profiles still run and generate logs/alerts, but any blocking action (reset-client, reset-server, reset-both, drop) has no effect on the actual traffic flow.

---

**Q2. Correct: C**

Virtual wire subinterfaces let you tag specific VLAN IDs on a vwire pair and map each tagged VLAN to a distinct zone. This provides Layer 2 transparency (no routing/topology change) while still allowing granular, VLAN-based security policy enforcement — a core vwire capability tested on the PCNSE.

---

**Q3. Correct: B**

PAN-OS evaluates Security policy using the original (pre-NAT) packet addresses/ports for matching, but uses the post-NAT destination zone because the zone lookup occurs after the NAT policy lookup determines the egress interface/zone. This mixed pre/post-NAT behavior is one of the most heavily tested packet-flow concepts on the PCNSE.

---

**Q4. Correct: C**

Single-pass architecture performs networking, policy lookup, App-ID classification, and content scanning (threat, URL, data filtering) in one integrated pass over the traffic stream, rather than making multiple sequential passes — this is the primary throughput/latency advantage over traditional multi-engine UTM designs.

---

**Q5. Correct: A**

Layer 2 deployment lets the firewall act as a transparent switch, forwarding frames based on MAC address within a VLAN object while enforcing security policy on inter-VLAN traffic passing through different zones — with no change to IP addressing, gateways, or routing.

---

**Q6. Correct: C**

PAN-OS enforces that a single security zone can only contain interfaces of the same deployment mode. Mixing a Layer 3 interface with virtual wire interfaces in the same zone is not permitted and will cause a validation error on commit.

---

**Q7. Correct: A**

PAN-OS resolves the NAT translation during session setup before Security policy is evaluated, then determines the destination zone based on the egress interface/zone required to reach the translated (post-NAT) address—in this case DMZ. However, the destination address field in the Security rule must still reference the original, pre-NAT destination address (203.0.113.10), since Security policy matches on pre-NAT addresses but post-NAT zones. Only option A reflects both rules correctly.

---

**Q8. Correct: B**

When only one specific site fails chain validation while all others decrypt cleanly, the root cause is almost always a missing intermediate CA certificate in the firewall's trusted CA store, which prevents the firewall from building a complete trust chain and forces it to sign the substitute certificate with the Forward Untrust CA instead of the Forward Trust CA.

---

**Q9. Correct: C**

NAT rule zone matching is based on the zone the original (pre-NAT) destination IP would route to according to the routing table, not the zone the traffic will ultimately land in after translation. Because 203.0.113.10 routes toward Untrust, a u-turn NAT rule must use source zone Trust and destination zone Untrust (matching that pre-NAT routing lookup), with source NAT added so the DMZ server's return traffic hairpins correctly back to the Trust host instead of routing to the internet.

---

**Q10. Correct: D**

The App-ID database documents facebook-chat as dependent on facebook-base, and facebook-base as dependent on web-browsing; PAN-OS does not automatically permit dependent applications through policy, so all three must be explicitly allowed (or covered by an application-group/filter) for facebook-chat to be permitted.

---

**Q11. Correct: C**

The documented best practice is to enable User Identification only on zones where user-to-IP mapping is actually needed for policy enforcement; the per-zone 'Enable User Identification' checkbox (Network > Zones) directly controls whether the firewall applies User-ID mapping within that zone, and disabling it on server/Data Center zones eliminates the spoofing risk described.

---

**Q12. Correct: D**

'Block sessions with untrusted issuer' lives on the SSL Forward Proxy tab and only applies when the firewall is validating an external server's certificate chain against its trusted CA store during Forward Proxy decryption. In SSL Inbound Inspection, the firewall already holds the actual server's private key and certificate, so there is no external issuer trust decision to evaluate, making this setting inapplicable.

---

**Q13. Correct: A**

PAN-OS ships with two hidden default Security policy rules: 'intrazone-default,' which allows all traffic within the same zone, and 'interzone-default,' which denies all traffic between different zones. Since both interfaces belong to the same DMZ zone, the intrazone-default allow rule is the one being matched, and it remains in effect until an administrator explicitly clones, modifies, or overrides it.

---

**Q14. Correct: D**

SSL Inbound Inspection works by using the server's private key to decrypt the RSA key exchange (the pre-master secret encrypted with the server's public key). When the server negotiates an ephemeral key exchange like ECDHE for Perfect Forward Secrecy, the session key is derived from ephemeral values that are never transmitted in a form the firewall can recover from the static private key. To support Inbound Inspection, the server must be configured to also offer (and the firewall/client negotiate) non-PFS cipher suites such as RSA key exchange, or PFS must be disabled on the server for that listener.

---

**Q15. Correct: B**

User-ID redistribution allows a firewall (or Panorama) that has already learned IP-to-user mappings to forward those mappings to other firewalls that lack direct visibility into the identity sources. FW-A is configured as the redistribution point, and FW-B is configured with a User-ID redistribution agent entry pointing to FW-A, receiving the already-resolved mappings without needing any direct connectivity to the domain controllers.

---

**Q16. Correct: D**

PAN-OS evaluates security policy zone matching using the original source zone but the post-NAT destination zone. Since the NAT rule translates the destination to an address that resides in the DMZ zone, the security policy must specify DMZ as the destination zone, not Untrust (the pre-NAT zone), or the session will be denied by the implicit deny rule due to a zone mismatch.

---

**Q17. Correct: C**

The Phishing verdict is a distinct WildFire classification used specifically for files or URLs identified as credential-phishing content, separate from the Malicious verdict used for malware behavior or exploits.

---

**Q18. Correct: D**

The DGA category in DNS Security is built to detect algorithmically generated domain names that malware families use to create large numbers of rotating C2 domains, defeating static blocklists.

---

**Q19. Correct: A**

After the DNS response is sinkholed, the infected host attempts to connect to the sinkhole IP address. Filtering the Traffic log for sessions destined to that IP reveals the actual source IP of the infected internal host.

---

**Q20. Correct: D**

The Exceptions tab in a Vulnerability Protection profile allows per-signature-ID action overrides, including block-ip with configurable track-by (source/destination) and duration, without altering the base rule's behavior for other threats.

---

**Q21. Correct: D**

When a site matches both a custom URL category and a PAN-DB predefined category within the same URL Filtering profile, the custom URL category's action takes precedence, so the block action is enforced.

---

**Q22. Correct: B**

PAN-OS Content-ID inspects traffic using a fixed internal engine order that begins with Vulnerability Protection (exploit/IPS signatures), independent of how profiles are arranged inside the security profile group.

---

**Q23. Correct: C**

When an internal DNS server forwards client queries to external resolvers, the firewall only sees the internal DNS server as the source of the DNS request it inspects, so the sinkhole log naturally records that server's IP. PAN-OS lets you list internal DNS server IP addresses in the Sinkhole configuration within the Anti-Spyware profile so the firewall recognizes these as forwarding DNS servers; it then correlates the subsequent client connection attempt to the sinkhole IP (which the infected host will make directly) back to identify the true source client, restoring visibility into which host is compromised.

---

**Q24. Correct: B**

Within a template stack, templates are evaluated top to bottom, and the template highest in the list order takes precedence for any overlapping configuration setting. Template_A sits at the top of Stack-Branch, so its MTU value wins.

---

**Q25. Correct: C**

Objects defined in a device group take precedence over objects with the same name defined in Shared. When Panorama pushes configuration, it overwrites any existing object of the same name on the target firewall, including a previously local, unmanaged object, replacing it with the pushed device group value.

---

**Q26. Correct: B**

The Enable Log Redundancy Across Collectors option in a Collector Group requires a minimum of two Log Collectors. With redundancy enabled, each log is forwarded to two Log Collectors in the group so that a single collector failure does not cause log loss.

---

**Q27. Correct: A**

Commit to Panorama only saves the candidate configuration to Panorama's own running configuration; it does not send anything to managed firewalls. A distinct Push to Devices operation, targeting the relevant device groups and templates, is required to deliver the configuration to the firewalls.

---

**Q28. Correct: C**

Template variables (prefixed with $) let an administrator define a placeholder value, such as an interface IP address, once in the shared template. Each firewall's entry in the template stack Variables tab can then override that placeholder with a device-specific value, enabling one template to serve many firewalls with unique settings.

---

**Q29. Correct: D**

The rule evaluation order in a Panorama-managed hierarchy is: Shared pre-rules, then device group pre-rules (ancestor to descendant), then local firewall pre-rules, then local firewall rules, then local post-rules, device group post-rules, and finally Shared post-rules. Because Shared pre-rules are evaluated first, the broader Shared deny rule matches the traffic before the DG-DataCenter pre-rule is ever reached.

---

**Q30. Correct: D**

Panorama resolves same-named objects by hierarchy proximity to the firewall: an object defined in any ancestor device group overrides a Shared object with the same name, and an object closer to the firewall (lower in the hierarchy) overrides one defined higher up. Since Region-DG defines WEB-SRV and no closer device group (Branch-DG or FW-DG) redefines it, the firewall receives Region-DG's value of 10.2.1.5, not the Shared value.

---

**Q31. Correct: A**

In active/active HA, HA3 is a dedicated Layer 2 link used exclusively to forward packets between peers when a session's traffic arrives at the non-owning firewall due to asymmetric routing, allowing the session owner to process it correctly.

---

**Q32. Correct: A**

PAN-OS automatically saves a configuration snapshot before an upgrade. To downgrade safely, the administrator must first ensure the installed content release is compatible with the older PAN-OS version (newer content packs can be incompatible with older releases), then downgrade and load the saved pre-upgrade config file to restore settings that may not survive the version change.

---

**Q33. Correct: A**

PAN-OS validates the full certificate chain from the client certificate up to a trusted root. If only the root CA is present in the certificate profile and the intermediate CA is missing, the chain cannot be built, causing valid certificates to fail validation even though they are not expired or revoked.

---

**Q34. Correct: B**

GlobalProtect supports generating an authentication cookie on the portal after successful authentication; when the gateway is configured to accept this cookie, the client can reconnect automatically within the cookie's validity period without re-prompting for credentials.

---

**Q35. Correct: A**

Enabling log redundancy in a Log Collector Group causes each log to be stored on two different Log Collectors instead of one, which protects against data loss if a collector fails, but it consumes roughly double the storage per log, cutting effective usable capacity by about half. Redundancy requires at least two Log Collectors in the group.

---

**Q36. Correct: A**

The Commit Lock (Configuration > Take Commit Lock) prevents any administrator from committing the candidate configuration until the lock is released, ensuring no one commits while another admin's edits are in progress — exactly the protection needed in this scenario.

---

**Q37. Correct: C**

HA3 is used exclusively in Active/Active mode as a Layer 2 packet-forwarding link. When a firewall that is not the session owner receives packets for that session (common with asymmetric routing), it forwards them over HA3 to the owning firewall for processing, rather than dropping them.

---

**Q38. Correct: B**

Palo Alto Networks requires that firewalls step through the base (x.x.0) image of every intermediate feature release when crossing multiple major/feature releases. From 10.1.x to 11.1.x, the device must install base images 10.2.0, 11.0.0, and 11.1.0 in order before installing the target maintenance release 11.1.2 — skipping a feature release's base image is not supported and can cause the upgrade to fail or the device to boot into an unexpected state.

---

**Q39. Correct: C**

When client certificates are issued by an intermediate/subordinate CA, the firewall must have that intermediate CA certificate imported and included in the Certificate Profile's CA Certificates list so it can build the full trust chain up to the trusted root. Having only the root CA in the profile is insufficient if the client cert chains through an intermediate that the firewall doesn't recognize.

---

**Q40. Correct: B**

PAN-OS performs NAT policy lookup before the security policy lookup to determine post-NAT zones. The security rule's destination zone must reflect the translated (post-NAT) zone, while the destination address in the rule still matches the original pre-NAT IP address because the packet header hasn't been rewritten yet at the point of policy evaluation. Using Untrust as the destination zone here is the misconfiguration.

---

**Q41. Correct: B**

The decrypt-cert-validation end-reason specifically indicates the firewall's Forward Proxy validation of the destination server's certificate failed (untrusted issuer, expired, or revoked) and the applicable Decryption Profile setting for blocking sessions with untrusted issuers/expired certificates is enabled, so the session is terminated rather than allowed through.

---

**Q42. Correct: A**

`show counter global filter delta yes packet-filter yes` restricts output to counters that have incremented since the packet-diag filter was set, which is the standard PAN-OS technique to pinpoint exactly which internal drop counter is firing for a matched flow without capturing full packets.

---

**Q43. Correct: D**

Certificate pinning — common in banking and financial mobile apps — causes the app to reject the firewall's re-signed certificate even though it is properly trusted at the OS level, because the app validates the server's original certificate specifically. Since only this one app fails while everything else decrypts fine, the documented remediation is a targeted decryption exclusion for that FQDN/app, not a CA, cipher, or protocol-wide change.

---

**Q44. Correct: B**

Certificate validation checks the SAN (not just CN) against the hostname used to connect; failing only on FQDN-based connections (not IP) strongly indicates a SAN/hostname mismatch, and the client must also trust the issuing internal CA. Both the SAN entry and CA trust chain must be correct for validation to succeed.

---

**Q45. Correct: D**

The symptom of the peer state reporting as "unknown" with no heartbeats received, despite the HA2 data link and dataplane being fully functional, is the classic signature of an HA1 control link failure. Without a configured HA1 backup link, both members lose heartbeat visibility and independently assume the active role, producing split-brain; the standard remediation is a redundant HA1 backup link on separate infrastructure.

---

**Q46. Correct: A**

The flow_fwd_l3_no_route counter is incremented during the forwarding lookup stage of the packet flow, which occurs after the initial parsing but is tied to the route lookup performed by the virtual router for that ingress zone. If there is no matching route in the routing table (including no default route), the firewall drops the packet at this stage -- before it ever reaches Security policy evaluation. This explains why flow_policy_deny stays at zero (policy was never evaluated) and why nothing shows in the Traffic log (a session was never established). The fix is to verify/add a route in the correct virtual router.

---

