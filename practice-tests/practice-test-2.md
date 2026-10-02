# Practice Test 2: PCNSE Exam Prep: Palo Alto Networks NGFW & Panorama

- **Questions:** 46
- **Time limit:** 90 minutes
- **Passing score:** 70%%

> Answers are in `answer-key-2.md`. Track results in `score-tracker.md`.

---

**Q1.** A firewall is connected to a switch SPAN port using a Tap interface for passive traffic visibility. The security team configures a Vulnerability Protection profile with the action set to 'Reset Both' and attaches it to a security policy rule matching the tapped traffic, expecting the firewall to terminate sessions matching known exploit signatures. What is the actual result?

A) The firewall sends TCP resets to both the client and server, terminating the malicious session as configured
B) The firewall generates threat logs for matched signatures, but no reset packets are sent because Tap mode only receives a copy of traffic and cannot inject packets into the flow
C) The reset action works only for UDP-based exploits when in Tap mode, not TCP
D) The firewall automatically converts the Tap interface to virtual wire to allow the reset action to function

---

**Q2.** A firewall is deployed in virtual wire mode between a core switch and a router. The organization has three VLANs (10, 20, 30) trunked across this link and requires each VLAN's traffic to be inspected and separated into its own security zone for distinct policy enforcement, without adding physical interfaces or changing the Layer 2/3 topology upstream. Which configuration achieves this?

A) Create three separate virtual routers, one per VLAN, on the same virtual wire pair
B) Convert the interfaces to Layer 3 subinterfaces with unique VLAN tags and enable inter-VLAN routing
C) Create virtual wire subinterfaces on the paired interfaces, each tagged with the corresponding VLAN ID, and assign each subinterface to its own zone
D) Enable a single zone across the virtual wire pair and rely on User-ID to separate VLAN traffic

---

**Q3.** A destination NAT rule translates the public address 203.0.113.10 (received on the Untrust zone) to the internal server 10.1.1.50 residing in the DMZ zone. When writing the corresponding Security policy rule to permit this traffic, which destination address and zone should the administrator reference?

A) Destination address 10.1.1.50 and destination zone DMZ (both post-NAT values)
B) Destination address 203.0.113.10 (pre-NAT) and destination zone DMZ (post-NAT)
C) Destination address 203.0.113.10 and destination zone Untrust (both pre-NAT values)
D) Destination address 10.1.1.50 (post-NAT) and destination zone Untrust (pre-NAT)

---

**Q4.** Which statement accurately describes a defining characteristic of PAN-OS's Single-Pass Parallel Processing (SP3) architecture as it relates to traffic classification and content inspection?

A) App-ID, User-ID, and Content-ID each perform independent full scans of the packet stream sequentially, and results are merged at the end of the pipeline
B) Full Layer 7 payloads must be reassembled and buffered into complete files before any classification can occur, similar to a proxy-based UTM
C) The packet is classified and inspected for threats, URLs, and data patterns in a single pass through the stream-based engine, avoiding the need to re-scan the same traffic multiple times for each feature
D) Each enabled security profile (Antivirus, Vulnerability Protection, URL Filtering) spins up a dedicated scanning process that runs in series, increasing per-packet latency proportionally to the number of profiles

---

**Q5.** A retail company wants the firewall to enforce security policy between three VLANs on the same physical segment, but does not want the firewall to perform any Layer 3 routing or change the existing IP addressing/gateway scheme — end hosts should continue to use their existing default gateway (a separate router). Which deployment model satisfies this requirement?

A) Layer 2 interfaces with VLAN objects, assigning each VLAN to its own zone so the firewall switches frames while enforcing policy between zones
B) Layer 3 interfaces with VLAN subinterfaces and OSPF enabled to route between VLANs
C) Virtual wire with a single zone spanning all VLANs
D) Tap mode with VLAN-based log forwarding filters

---

**Q6.** An administrator has a security zone named 'DC-VirtualWire' containing two virtual wire interfaces. They attempt to add a Layer 3 interface (ethernet1/5) to this same zone to consolidate policy management. What happens when they try to commit this configuration?

A) The commit succeeds, and the zone now enforces policy across both the virtual wire pair and the Layer 3 interface transparently
B) The commit succeeds only if both interfaces are placed in the same virtual router
C) The commit fails or the interface is rejected from the zone because a security zone can only contain interfaces of a single, matching deployment type (e.g., all virtual wire, all Layer 2, or all Layer 3)
D) The commit succeeds, but only tap interfaces are permitted to coexist with virtual wire interfaces in the same zone

---

**Q7.** A firewall has ethernet1/1 bound to zone "Untrust" and ethernet1/2 bound to zone "DMZ". A Destination NAT rule translates public address 203.0.113.10 to private address 10.1.1.50, which is reachable via ethernet1/2. An administrator writes a Security policy rule with source zone Untrust, destination zone Untrust, and destination address 203.0.113.10 (the original, pre-NAT address). Traffic to the server is being dropped. Which single change to the Security policy rule will allow this traffic while following correct PAN-OS zone and NAT evaluation logic?

A) Change the destination zone to DMZ, keeping the destination address as the original pre-NAT address 203.0.113.10
B) Change the destination address to the translated address 10.1.1.50, keeping the destination zone as Untrust
C) Change the destination zone to 'Any' so the rule matches regardless of which interface receives the translated packet
D) Move the Security rule above the NAT rule in the rulebase so zone lookup occurs before translation is applied

---

**Q8.** A firewall administrator enables SSL Forward Proxy decryption for all outbound web traffic. Users report that when accessing a specific banking site that presents a valid, publicly trusted certificate, they still receive a browser certificate-warning page. Firewall decryption logs show the site's certificate chain terminating at an untrusted issuer, while all other HTTPS sites decrypt without warnings. What is the MOST likely cause and correct fix?

A) The Decryption Profile is configured to block sessions with expired certificates, and the banking site's certificate has expired
B) The firewall's trusted CA store is missing the intermediate CA certificate used by the banking site; import the missing intermediate certificate under Device > Certificate Management > Certificate Authority
C) The banking site requires client certificate authentication, which SSL Forward Proxy does not support, causing the firewall to substitute the Forward Untrust certificate
D) The Forward Trust certificate has expired; regenerate and re-push the Forward Trust CA to all managed endpoints

---

**Q9.** A web server sits in the DMZ zone at 10.1.1.10 and is published externally as 203.0.113.10 via a static Destination NAT rule with source zone 'Untrust' and destination zone 'Untrust' (the zone associated with the public IP's routing). Internal users in the 'Trust' zone try to reach the server using the public IP 203.0.113.10, and connections fail, even though a Security policy rule permits Trust-to-DMZ traffic. Which NAT configuration change correctly enables this u-turn NAT scenario?

A) Change the original NAT rule's destination zone from 'Untrust' to 'Trust', since the server physically resides behind an interface reachable from Trust
B) Create a security policy rule allowing Trust to Untrust for the public IP; NAT rules do not require additional zone entries for u-turn traffic since zone lookup always uses the post-NAT address
C) Add a second NAT rule with source zone 'Trust' and destination zone 'Untrust' (matching the zone the pre-NAT destination IP 203.0.113.10 routes to), keeping the same destination translation to 10.1.1.10, and add source NAT to hairpin the return traffic back through Trust
D) Add a source NAT rule that translates the Trust users' source IP to the DMZ interface IP while leaving the NAT rule's destination zone set to 'DMZ'

---

**Q10.** According to Palo Alto Networks' documented App-ID application dependency chain, which additional applications must an administrator explicitly permit in the Security policy to allow 'facebook-chat' to function correctly?

A) facebook-base only, because web-browsing is validated automatically at Layer 7 without requiring an explicit policy match
B) No additional applications are required; App-ID automatically permits all application dependencies once the child application is allowed in policy
C) ssl and web-browsing only, since facebook-chat depends solely on the encrypted transport protocols it rides on
D) facebook-base and web-browsing, because facebook-chat depends on facebook-base, which itself depends on web-browsing

---

**Q11.** A security engineer notices that the User-ID feature is actively mapping IP addresses to usernames inside the Data Center zone, where servers communicate using shared service accounts. This produces misleading user attribution in traffic logs and increases the risk of user-mapping spoofing from that zone. What is the recommended configuration to correct this?

A) Disable User-ID entirely on the firewall, since server-to-server traffic never benefits from identity-based enforcement anywhere on the platform
B) Configure the Zone Protection Profile applied to the Data Center zone to disable User-ID enforcement, since Zone Protection Profiles control User-ID scope
C) Enable User-ID only on the zones that require user-to-IP mapping (such as end-user access zones), leaving 'Enable User Identification' unchecked on the Data Center zone
D) Enable 'Include/Exclude Networks' on the User-ID agent to exclude the Data Center subnet from being probed

---

**Q12.** An administrator configures SSL Inbound Inspection to decrypt and inspect traffic destined to an internal application server, using a Decryption Profile that has settings enabled on both the 'SSL Protocol Settings' tab and the 'SSL Forward Proxy' tab. Which single setting will have NO effect on the SSL Inbound Inspection sessions?

A) Block sessions with unsupported cipher suites, under SSL Protocol Settings
B) Block sessions if the resumed handshake reuses a session ticket, under SSL Protocol Settings
C) Block sessions with expired certificates, under SSL Protocol Settings
D) Block sessions with untrusted issuer, located on the SSL Forward Proxy tab

---

**Q13.** Two interfaces, ethernet1/3 and ethernet1/4, are both assigned to the same zone, 'DMZ', on a Palo Alto Networks firewall. No explicit Security policy rules have been created for DMZ-to-DMZ traffic. An administrator observes that traffic between hosts on these two interfaces is being permitted anyway. What explains this behavior?

A) The built-in 'intrazone-default' Security policy rule allows all traffic within the same zone by default, unless explicitly modified or overridden by an administrator
B) Because App-ID has not yet fully identified the application, the session default action is allow until identification completes, causing the traffic to pass in the interim
C) Layer 3 zones automatically create an implicit allow rule for any pair of interfaces sharing the same zone, independent of the default rule set
D) The firewall's built-in 'interzone-default' rule allows all traffic within the same zone by default

---

**Q14.** A network security engineer configures SSL Inbound Inspection on the firewall to decrypt traffic to an internal web server. The server's certificate and private key are imported correctly, and a decryption policy matches the traffic. However, packet captures show the firewall is not decrypting the sessions, and traffic logs show 'decrypt-cert-validation' style failures are absent — decryption simply isn't occurring. The server's SSL configuration shows it prefers ECDHE_RSA cipher suites. What is the most likely cause?

A) The decryption profile does not have TLS 1.2 enabled under SSL Protocol Settings
B) The imported server certificate is missing the intermediate CA in its chain
C) The interface is deployed in virtual wire mode, which does not support SSL Inbound Inspection
D) The server negotiates ECDHE (Perfect Forward Secrecy) key exchange, so the firewall cannot derive the session key from the imported RSA private key alone

---

**Q15.** A firewall (FW-B) in a remote data center segment has no network path to the corporate domain controllers used for User-ID mapping, but security policies on FW-B require the same IP-address-to-username mappings that FW-A at headquarters has already collected via WMI and Windows-based agents. Which configuration correctly provides FW-B with these mappings?

A) Configure FW-B as a User-ID agent to receive syslog messages directly from the domain controllers
B) Configure User-ID redistribution: designate FW-A as the redistribution point and configure FW-B to receive user mappings from FW-A
C) Configure Group Mapping via LDAP on FW-B pointing to the headquarters domain controllers
D) Install a standalone Windows-based User-ID agent on FW-B and point it at the headquarters domain controllers

---

**Q16.** An internal host in the Trust zone resolves a company web server using its public DNS name and attempts to connect to the resulting public IP address. A U-turn NAT rule is configured to translate the destination address from the public IP to the internal server's real IP address in the DMZ zone. The security policy rule permitting this traffic is configured with source zone Trust and destination zone Untrust, matching the original (pre-NAT) destination. The connection fails. What must be corrected?

A) The security policy's source zone must be changed to Untrust, since the destination address is a public IP
B) A second NAT rule must be created explicitly for the return traffic from the DMZ server back to the internal host
C) A static route must be added directing the public IP address toward the Trust zone interface
D) The security policy's destination zone must be changed to match the post-NAT zone (DMZ), since PAN-OS evaluates zone matching using the post-NAT destination zone

---

**Q17.** An analyst reviews WildFire submission logs and notices a PDF file was assigned a verdict of 'Phishing' rather than 'Malicious' or 'Grayware'. What does this specific WildFire verdict indicate about the file?

A) The file exhibited command-and-control callback behavior inside the WildFire sandbox
B) The file matched a known malware family signature in the WildFire signature database
C) The file contains embedded links or content designed to steal user credentials
D) The file contains a zero-day exploit targeting a known CVE

---

**Q18.** Which DNS Security category is specifically designed to identify domain names produced by malware algorithms used to generate rotating, resilient command-and-control infrastructure?

A) Grayware Domains
B) Parked Domains
C) Newly Registered Domains
D) Domain Generation Algorithms (DGA)

---

**Q19.** An Anti-Spyware profile uses the DNS Sinkhole action with the default Palo Alto Networks sinkhole FQDN. When an internal host queries a malicious domain, the firewall returns the sinkhole IP address in the DNS response. To identify which internal host is actually infected and attempting to reach the malicious domain, which log should the administrator examine?

A) The Traffic log filtered on destination IP equal to the sinkhole IP address
B) The System log filtered for DNS proxy events
C) The WildFire Submissions log for the resolved domain name
D) The URL Filtering log filtered for blocked categories

---

**Q20.** A Vulnerability Protection profile's base rule uses the 'default' action for all critical-severity threats. The security team wants threat ID 39900 specifically to trigger a block-ip action with a 300-second duration, tracked by source, while every other critical threat continues using the default action. How should this be configured?

A) Add a custom vulnerability signature that duplicates threat ID 39900 with a higher-priority action
B) Create a separate Security policy rule matching threat ID 39900 and set the action to Deny
C) Modify the base rule's action to block-ip and configure the tracking duration under the profile's Rules tab
D) Create an Exception entry for threat ID 39900 in the profile, set its action to block-ip, and configure track-by and duration under the Exceptions tab

---

**Q21.** A custom URL category named 'Finance-Block' contains a set of specific FQDNs and is set to action 'block' in a URL Filtering profile. Those same FQDNs also belong to the PAN-DB predefined category 'financial-services', which is set to action 'alert' in the same profile. When a user browses to one of these FQDNs, which action does the firewall enforce?

A) continue, because conflicting actions between categories default to the least restrictive setting
B) alert, because PAN-DB predefined categories always take precedence over custom URL categories
C) The firewall enforces both actions simultaneously, logging an alert and blocking the session
D) block, because custom URL categories take precedence over PAN-DB predefined categories

---

**Q22.** A traffic flow matches a security profile group containing Antivirus, Anti-Spyware, Vulnerability Protection, and URL Filtering profiles, each configured to block on a match. Regardless of the order in which the profiles are listed within the profile group, which threat engine evaluates the traffic FIRST?

A) Antivirus
B) Vulnerability Protection (IPS)
C) Anti-Spyware
D) URL Filtering

---

**Q23.** A firewall enforces DNS sinkholing via the Anti-Spyware profile's DNS Signature Policies. Internal clients send all DNS queries to an internal DNS server, which caches responses and forwards unresolved queries to a public DNS resolver on the internet; the firewall only inspects the traffic between the internal DNS server and the internet. After enabling the sinkhole action, every Threat log entry for a DNS sinkhole event shows the internal DNS server's IP as the source address, regardless of which client actually issued the malicious query. This makes it impossible to identify the infected host from the DNS log alone. Which change resolves this visibility problem without disabling DNS sinkholing?

A) Change the DNS signature action from 'sinkhole' to 'block' so the source IP is logged directly
B) Enable User-ID redistribution between the firewall and the internal DNS server's zone
C) Add the internal DNS server's IP address to the 'Internal DNS servers' list in the Sinkhole tab of the Anti-Spyware profile
D) Configure the security policy rule matching DNS traffic to log at session start instead of session end

---

**Q24.** A Panorama template stack named Stack-Branch contains three templates in this order (top to bottom): Template_A, Template_B, Template_C. All three templates configure a different MTU value on the same subinterface. When the stack is pushed to a managed firewall, which template's MTU value is applied to the firewall?

A) Template_B, because middle templates always override boundary templates
B) Template_A, because the topmost template in the stack has the highest priority
C) Template_C, because the bottommost template is applied last and therefore wins
D) Whichever template was added to the stack most recently, regardless of its position

---

**Q25.** An administrator creates an address object named WebServer under Shared with value 10.1.1.10. The same name, WebServer, is also defined inside device group DG-DataCenter with value 10.2.2.20. A firewall assigned to DG-DataCenter already has a locally configured address object named WebServer with value 192.168.1.5 that was never managed by Panorama. After the administrator commits to Panorama and pushes to the device group, what happens to the WebServer object on that firewall?

A) The Shared value (10.1.1.10) is used because Shared objects always have the highest precedence over device group objects
B) The push operation fails with a duplicate-object validation error, and the administrator must rename one of the objects before pushing again
C) The device group value (10.2.2.20) overrides the Shared value, and the push overwrites the local firewall object of the same name with 10.2.2.20
D) The local firewall value (192.168.1.5) is retained because locally defined objects can never be overwritten by a Panorama push

---

**Q26.** An administrator is configuring a Collector Group with three Dedicated Log Collectors and wants to enable log redundancy so that no logs are lost if a single Log Collector goes offline. What is the minimum number of Log Collectors required in the Collector Group to enable the log redundancy option?

A) Exactly one Log Collector, provided it is configured with a RAID disk pair
B) Two Log Collectors
C) Log redundancy is enabled by default and requires no minimum Log Collector count
D) At least four Log Collectors to form a quorum

---

**Q27.** An administrator clicks Commit in Panorama and selects Commit to Panorama. The commit job reports Commit succeeded. Several minutes later, the managed firewalls in device group DG-Branch are still enforcing the old policy. What should the administrator do to apply the new configuration to the firewalls?

A) Perform a separate Push to Devices operation targeting the DG-Branch device group and associated templates
B) Increase the commit timeout value in Panorama Setup settings and reissue Commit to Panorama
C) Manually export the configuration and import it into each firewall through its local Web UI
D) Restart the Panorama management service to force synchronization with managed devices

---

**Q28.** A managed security team is building one template to standardize the configuration of 40 branch firewalls, but each firewall needs a unique IP address assigned to interface ethernet1/1, which differs per site. Which approach lets the team use a single shared template while still assigning a unique interface IP address to each firewall?

A) Create a separate template for every firewall model and disable variables entirely
B) Configure the interface IP address directly under Device Group > Objects > Addresses for each firewall
C) Define a variable for the interface IP address in the template, then override its value per firewall under the template stack Variables tab
D) Use a Log Forwarding profile to dynamically assign interface IP addresses at commit time

---

**Q29.** Device group DG-DataCenter has a pre-rulebase security rule that permits traffic between two internal subnets. Traffic matching this rule is unexpectedly being denied. Investigation reveals a Shared pre-rule in Panorama with a broader match and a deny action. Which statement correctly explains the rule evaluation order that produces this behavior?

A) Local firewall pre-rules are always evaluated before shared or device group pre-rules, so the deny should not be occurring
B) Device group post-rules are evaluated before shared pre-rules, so the shared rule executes too late to cause a deny
C) Rule evaluation order is alphabetical by rule name across all rulebases, regardless of shared or device group scope
D) Shared pre-rules are evaluated before device group pre-rules in the rule hierarchy, so the broader shared deny rule matches the traffic first

---

**Q30.** A Panorama device group hierarchy is structured as: Shared (top) > Region-DG > Branch-DG > FW-DG (contains the firewall). An address object named "WEB-SRV" is defined in Shared with IP 10.1.1.5. The same object name "WEB-SRV" is also defined in Region-DG with IP 10.2.1.5. No object with that name exists in Branch-DG or FW-DG. After a commit and push to the device group, which IP address does the firewall in FW-DG actually receive for the object "WEB-SRV"?

A) 10.1.1.5, because Shared objects always take precedence over device group objects to ensure global consistency
B) The push fails with a commit error because of a naming conflict between Shared and Region-DG
C) Both values are merged into an address group automatically applied to the firewall
D) 10.2.1.5, because the object defined in the closest ancestor device group in the hierarchy overrides the Shared object of the same name

---

**Q31.** A firewall pair is deployed in active/active HA. Due to asymmetric routing upstream, return traffic for a session sometimes arrives at the non-owning firewall. The security policy has 'Forward packets to session owner' behavior expected, but sessions are being dropped instead. Which HA component should the administrator verify is correctly configured to resolve this?

A) The HA3 packet-forwarding link, since it forwards packets to the session owner for asymmetric traffic
B) The HA1 control link, since it carries heartbeat and state synchronization messages
C) The HA2 link, since it synchronizes session table entries between the active peers
D) The HA1-backup link, since it provides redundancy for HA state election

---

**Q32.** An administrator upgrades a firewall from PAN-OS 11.1 to 10.2 after encountering stability issues, but wants to preserve as much of the pre-upgrade configuration as possible and avoid content pack incompatibility errors. What is the correct approach?

A) Install a content release version supported by PAN-OS 10.2 before downgrading, then downgrade and load the autosaved pre-upgrade configuration snapshot
B) Downgrade directly; PAN-OS automatically reverts both the configuration and content versions to match the target release
C) Perform a factory reset, downgrade, then manually re-enter the entire configuration from documentation
D) Export the running configuration as XML immediately before downgrading and re-import it after the downgrade completes

---

**Q33.** GlobalProtect gateway authentication uses client certificates issued by an enterprise intermediate CA. Users with valid, unexpired certificates fail authentication with certificate validation errors. The root CA certificate is correctly installed in the certificate profile. What is the most likely cause?

A) The intermediate CA certificate is missing from the certificate profile's CA chain, breaking the trust path validation
B) The OCSP responder for the CA is temporarily unreachable, causing hard-fail certificate rejection
C) The client certificates were issued using SHA-1 instead of SHA-256
D) The GlobalProtect portal's server certificate has expired

---

**Q34.** Mobile users frequently lose GlobalProtect connectivity due to unstable Wi-Fi and cellular handoffs. The security team wants users to automatically reconnect without re-entering credentials for a limited, defined period after the initial successful authentication. Which configuration achieves this?

A) Set the Connect Method to 'Pre-logon (Always On)' on the GlobalProtect app configuration
B) Enable 'Generate cookie for authentication override' on the portal and accept the cookie on the gateway, with a defined cookie lifetime
C) Enable 'Save User Credentials' on the GlobalProtect portal agent configuration
D) Increase the SSL/TLS session resumption timeout on the portal's SSL/TLS service profile

---

**Q35.** A Panorama Log Collector Group contains three Log Collectors. The administrator enables log redundancy for the group. What is the direct operational impact of this change?

A) Each log entry is written to two separate collectors, reducing the group's effective usable storage capacity by roughly half
B) Log throughput triples because each collector processes logs independently in parallel
C) Log redundancy has no impact on storage, only on query performance during collector failover
D) The Log Collector Group automatically converts to a Panorama HA pair for log storage

---

**Q36.** Two administrators are working on the same firewall's candidate configuration simultaneously: Admin A is midway through modifying several security policy rules, while Admin B is preparing to commit unrelated NAT changes. To prevent Admin B from committing while Admin A's edits are incomplete, thereby avoiding an inconsistent partial configuration push, which action should Admin A take before starting the edits?

A) Take the Commit Lock from the Configuration menu to block any commit by other administrators until released
B) Apply an Object Lock on the specific security policy rules being edited
C) Disable Admin B's account temporarily via Device > Administrators
D) Save a named configuration snapshot before editing begins

---

**Q37.** A Palo Alto Networks firewall pair is configured in Active/Active HA mode across two ISP uplinks. Due to asymmetric routing, return traffic for some sessions arrives at the firewall that did NOT establish the original session. Which HA link is responsible for forwarding these packets to the firewall that owns the session?

A) HA1 (control link)
B) HA2 (state synchronization link)
C) HA3 (packet forwarding link)
D) The dedicated management (MGT) interface

---

**Q38.** A firewall running PAN-OS 10.1.9 must be upgraded to PAN-OS 11.1.2 to meet a compliance deadline. Following Palo Alto Networks' official upgrade path requirements, which sequence of installations is correct?

A) Directly download and install PAN-OS 11.1.2 in a single step
B) Install the 10.2.0 base image, then the 11.0.0 base image, then the 11.1.0 base image, then install the target 11.1.2 release
C) Install PAN-OS 11.0.0 directly, then upgrade straight to 11.1.2
D) Install the latest content updates first, then jump directly from 10.1.9 to 11.1.2

---

**Q39.** Users connecting to a GlobalProtect gateway with certificate-based client authentication receive an 'untrusted certificate' error, even though the client certificates were issued by the organization's internal enterprise CA. The Certificate Profile on the gateway already includes the enterprise Root CA certificate. Troubleshooting reveals the client certificates were issued by a subordinate (intermediate) CA under that root. What should the engineer do to resolve the issue?

A) Change the GlobalProtect gateway authentication method from certificate-based to SAML
B) Enable Certificate Revocation List (CRL) checking on the client certificate profile
C) Import the intermediate CA certificate onto the firewall and add it to the CA Certificates list in the GlobalProtect certificate profile
D) Regenerate the SSL/TLS Service Profile certificate used by the GlobalProtect portal

---

**Q40.** A destination NAT rule translates traffic addressed to public IP 203.0.113.10 to an internal server at 10.1.1.50, which resides in the DMZ zone (interface ethernet1/3, DMZ zone). The Untrust zone interface faces the internet. An engineer configures a security policy rule with source zone Untrust, destination zone Untrust, destination address 203.0.113.10, action allow. Traffic matching the NAT rule is still denied. Given how PAN-OS orders NAT and security policy evaluation, what is the root cause?

A) Security policy is evaluated first using the pre-NAT destination zone, so Untrust is correct; the deny is caused by an unrelated App-ID dependency mismatch, not zone selection.
B) NAT policy lookup occurs before the security policy lookup and determines that the packet's destination zone for policy purposes is DMZ (the zone of the translated address); the security rule must use DMZ as the destination zone while still matching the original pre-NAT address 203.0.113.10.
C) Destination NAT rules automatically create an implicit security allow, so the manual rule is redundant; the real problem is a missing route to 10.1.1.50 in the virtual router.
D) The security rule needs a corresponding NAT-exempt security policy for the return traffic; without a second explicit rule for 10.1.1.50 to Untrust, the session is denied on the reply packet.

---

**Q41.** While troubleshooting SSL Forward Proxy decryption, `show session id <id>` on a firewall returns: session end-reason: decrypt-cert-validation. All other decrypted sites work normally. Which situation most likely produced this specific end-reason?

A) The client browser does not trust the firewall's Forward Trust CA certificate, producing an in-browser certificate warning.
B) The destination server presented a certificate that failed validation against the firewall's trusted CA store (for example, expired, self-signed, or revoked), and the Decryption Profile is configured to block sessions with untrusted server certificates.
C) The negotiated TLS version between client and server (TLS 1.3) is unsupported by the decryption profile in use.
D) The client presented an expired client certificate during a mutual TLS handshake with the destination server.

---

**Q42.** An engineer suspects packets destined for an internal host are being silently dropped at some internal processing stage but wants to avoid running a full packet capture. Which command displays only the global counters that have incremented since the packet-diag filter was applied, allowing quick identification of the exact drop reason (e.g., flow_policy_deny, flow_fwd_l3_noroute)?

A) show counter global filter delta yes packet-filter yes
B) debug dataplane packet-diag show setting
C) show session all filter destination 10.1.1.50
D) tcpdump filter "host 10.1.1.50" view-pcap

---

**Q43.** Users report that a specific mobile banking application consistently fails to connect whenever SSL Forward Proxy decryption is enabled for their zone. The decryption policy correctly matches the app's traffic, and the Forward Trust CA certificate is properly deployed and trusted on all managed endpoints. Disabling decryption for that traffic immediately resolves the issue, while all other decrypted sites continue working fine. What is the most likely cause and best remediation?

A) The Forward Trust certificate has expired, so it must be regenerated and redeployed to all endpoints before any decrypted TLS 1.2 session will succeed.
B) The application requires TLS 1.3 with 0-RTT, which PAN-OS cannot decrypt under any circumstance, so all TLS 1.3 destinations must be globally excluded from decryption.
C) The Decryption Profile is blocking sessions using unsupported cipher suites; enabling "Allow client to use unsupported cipher suites" will resolve the failure.
D) The application uses certificate pinning, validating the exact server certificate rather than trusting the OS/browser certificate store; create a decryption exclusion (no-decrypt policy or SSL Decryption Exclusion entry) for the application's specific FQDN.

---

**Q44.** Remote users see the error "Unable to connect to GlobalProtect service: Verify server certificate" only when connecting to the gateway using its published FQDN (not when connecting directly by IP). The gateway's SSL/TLS Service Profile uses a certificate issued by an internal enterprise CA, and the GlobalProtect app is configured to validate server certificates. What is the most likely fix?

A) Switch the gateway's authentication profile from certificate-based authentication to SAML, which removes any requirement to validate the gateway's server certificate.
B) Ensure the gateway certificate's Subject Alternative Name (SAN) includes the FQDN used to reach the gateway, and that the issuing enterprise CA's certificate chain is included in the client's trusted root store or Certificate Profile.
C) Temporarily disable "Validate Server Certificate" in the GlobalProtect app configuration and leave it disabled, since internal CAs are not supported by the GlobalProtect app.
D) Regenerate the gateway certificate using a 4096-bit key instead of 2048-bit, since GlobalProtect requires stronger keys when connecting via FQDN rather than IP.

---

**Q45.** In an active/passive HA pair, both firewalls unexpectedly transition to the active state simultaneously, causing routing instability. The HA2 (data) link and all dataplane interfaces remain fully operational between sites, but `show high-availability state` on each firewall reports the peer as "unknown" with no heartbeats received. What most likely caused this split-brain condition, and what is the recommended long-term fix?

A) The two firewalls have identical HA device priority values, causing an active/active race condition; assign distinct explicit device priorities to each firewall.
B) The HA group ID differs between the two firewalls, causing them to form two independent HA clusters that were never actually paired; correct the group ID so both peers match.
C) Path monitoring for a downstream link-monitored destination failed on the passive firewall, triggering unwanted preemption; disable preemption to resolve the issue.
D) The HA1 (control) link failed with no configured HA1 backup link, so each firewall independently lost contact with its peer's heartbeat and assumed the other had failed; configure a redundant HA1 backup link over a separate physical/network path.

---

**Q46.** A security policy explicitly permits traffic from Zone-Trust to a server at 10.10.50.5, but the traffic never appears in the Traffic log, and the connection times out. Running 'show counter global filter packet-filter yes delta yes' while regenerating traffic shows the counter 'flow_fwd_l3_no_route' rapidly incrementing, while 'flow_policy_deny' remains at zero. What is the MOST likely root cause?

A) The virtual router associated with the ingress zone has no route to 10.10.50.5, so the packet is dropped before policy lookup completes
B) The Security policy rule is missing a Log Forwarding profile, so the session is processed but never recorded
C) A Decryption policy is silently blocking the session before the Security policy can be evaluated
D) The destination NAT translation in the NAT policy is pointing to the wrong zone, causing a security policy mismatch

---

