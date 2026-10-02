# Practice Test 1: PCNSE Exam Prep: Palo Alto Networks NGFW & Panorama

- **Questions:** 46
- **Time limit:** 90 minutes
- **Passing score:** 70%%

> Answers are in `answer-key-1.md`. Track results in `score-tracker.md`.

---

**Q1.** A network architect must insert a Palo Alto Networks firewall between an existing core switch and edge router to enable App-ID, Content-ID, and threat prevention, but the customer's change window prohibits any IP address or routing changes to the existing topology. Which interface deployment mode BEST meets these requirements?

A) Tap mode
B) Layer 3
C) Layer 2 with VLAN trunking
D) Virtual wire

---

**Q2.** A SOC wants to evaluate a new Palo Alto Networks firewall's threat detection accuracy by connecting it to a SPAN port on a core switch. The firewall must not be able to drop or modify any production traffic during this evaluation. Which interface type should be configured?

A) Virtual wire
B) Layer 3 subinterface
C) Tap
D) Layer 2

---

**Q3.** Which statement BEST describes the primary architectural advantage of PAN-OS Single-Pass Parallel Processing (SP3) compared to legacy multi-pass UTM firewalls?

A) It uses signature-only detection to reduce CPU load
B) It requires separate proxy engines for each application protocol
C) It scans traffic multiple times using dedicated hardware ASICs for each security function
D) Traffic is classified once and App-ID, User-ID, Content-ID, and policy enforcement all operate on that single classification, reducing latency and avoiding redundant packet processing

---

**Q4.** An administrator creates a new zone named DMZ-Tap with Zone Type set to Layer3, then attempts to assign a Tap-mode interface to that zone. What is the result?

A) The assignment succeeds and traffic is inspected using Layer 3 security policies
B) PAN-OS automatically converts the zone type to Tap
C) The commit fails because the zone type must match the interface type
D) The interface is assigned but generates only informational log entries

---

**Q5.** An organization has a segment that must remain a transparent Layer 2 broadcast domain (no IP re-addressing of hosts) but requires multiple VLANs to be inspected separately with distinct security zones on the same physical interface. Which interface configuration should be used?

A) A single Layer 3 interface with sub-interfaces per VLAN
B) Virtual wire with VLAN tag matching
C) Layer 2 interface with VLAN sub-interfaces mapped to separate zones
D) Tap interface with VLAN mirroring

---

**Q6.** A firewall receives the first packet of a new TCP session on port 443. At this point in packet flow, PAN-OS has not yet identified the actual application via App-ID. Which statement correctly describes how the security policy lookup behaves in this scenario?

A) The session is dropped until App-ID completes identification
B) The initial policy match uses zone, IP, and port information (treating the app as unknown), and once App-ID identifies the real application, PAN-OS re-evaluates the security policy match against that identified application
C) PAN-OS applies the policy rule matched by service port permanently for the life of the session regardless of later App-ID results
D) The firewall bypasses security policy entirely for the first packet and only applies it from the second packet onward

---

**Q7.** A network security engineer configures a Destination NAT rule on a PAN-OS firewall that translates traffic destined for public IP 203.0.113.10 (residing in the Untrust zone) to an internal web server at 10.1.1.100 (residing in the Trust zone). When the engineer creates the corresponding Security policy rule to permit this traffic, which destination zone and destination IP address should be specified?

A) Destination zone: Trust; Destination IP: 10.1.1.100
B) Destination zone: Untrust; Destination IP: 203.0.113.10
C) Destination zone: Untrust; Destination IP: 10.1.1.100
D) Destination zone: Trust; Destination IP: 203.0.113.10

---

**Q8.** A company must decrypt outbound HTTPS traffic on the firewall to inspect for threats, but cannot tolerate certificate-trust warnings on any of its 5,000 managed Windows endpoints. Which approach BEST meets this requirement?

A) Enable SSL Forward Proxy using the firewall's built-in default self-signed certificate
B) Generate a subordinate CA certificate signed by the enterprise root CA, import it into the firewall as the Forward Trust certificate, and rely on the enterprise root CA already trusted by endpoints
C) Configure SSL Inbound Inspection using the server's certificate and private key
D) Leave decryption disabled and depend solely on App-ID and Content-ID signatures

---

**Q9.** Internal users on the corporate LAN resolve an internal server's FQDN to its public NAT IP address (split-horizon DNS is not used), and must reach that server successfully from inside the network. Which NAT configuration BEST satisfies this requirement?

A) A standard Destination NAT rule only, translating the public IP to the private server IP
B) Dynamic IP and Port (DIPP) Source NAT on the internal zone
C) U-Turn NAT: a Destination NAT rule with source zone and destination zone both set to the internal zone, translating destination to the internal server's private IP
D) A NAT exemption rule that disables NAT for internal-to-internal traffic

---

**Q10.** A homegrown internal application is being misidentified as 'insufficient-data' by App-ID, causing inconsistent Security policy enforcement. The security team wants App-ID to accurately and consistently identify this application by its actual traffic pattern, while still allowing Content-ID inspection (threat, URL filtering) to continue running on that traffic. Which configuration BEST meets this requirement?

A) Create an Application Override policy mapping the application's IP address and port to a custom app name
B) Request Palo Alto Networks to release a new App-ID signature in the next content update
C) Create a Custom Application with a signature based on context and pattern match unique to the application's traffic
D) Apply a Vulnerability Protection profile tuned to detect the application's traffic signature

---

**Q11.** Which statement correctly describes how PAN-OS evaluates rules within a single Security policy rulebase for a new session?

A) Rules are evaluated from top to bottom, and the first rule that matches the session's criteria is applied, stopping further evaluation
B) Rules are evaluated in alphabetical order by rule name, and the first alphabetical match is applied
C) All matching rules are applied cumulatively, with the most restrictive action taking precedence
D) Rules are evaluated based on a numeric priority tag, independent of their position in the rulebase

---

**Q12.** A company runs a Citrix XenApp/RDS environment where dozens of users share the same source IP address for their published application sessions. Security policy must map traffic accurately to individual users for User-ID-based rule enforcement. Which User-ID configuration BEST fits this environment?

A) Agentless User-ID with WMI probing configured against each end-user workstation
B) The standard Windows-based User-ID agent monitoring Active Directory domain controller security event logs
C) The PAN-OS Terminal Server (TS) Agent installed on the Citrix/RDS server, assigning a dedicated port range per user session
D) Captive Portal applied to all traffic sourced from the Citrix server's IP address

---

**Q13.** An organization wants the firewall to decrypt and inspect inbound HTTPS traffic destined to an internal web server that the organization owns and controls. Which configuration BEST accomplishes this?

A) Generate a subordinate CA certificate on the firewall and enable SSL Forward Proxy for inbound traffic
B) Import the web server's actual certificate and private key onto the firewall, then create a Decryption policy rule using SSL Inbound Inspection
C) Configure a 'No Decrypt' rule with detailed traffic logging enabled for the inbound zone
D) Enable Forward Trust and Forward Untrust certificates and apply them to the inbound Decryption rule

---

**Q14.** A company wants to decrypt outbound SSL/TLS traffic from managed corporate laptops to inspect it for threats, without generating browser certificate-trust warnings for end users. Which approach BEST meets this requirement?

A) Use the default Palo Alto Networks self-signed CA certificate that ships with PAN-OS as the Forward Trust certificate
B) Purchase a publicly trusted CA-signed certificate (e.g., from a commercial CA like DigiCert) and configure it as the Forward Trust certificate
C) Generate a self-signed certificate on the firewall and manually install it in the trusted root store on each laptop individually
D) Obtain a subordinate CA certificate from the enterprise PKI and configure it as the Forward Trust certificate on the firewall

---

**Q15.** Multiple users share a single source IP address when connecting through a Windows Terminal Server, and the security team needs User-ID to accurately map individual user sessions to that shared IP for policy enforcement. Which User-ID method should be configured?

A) PAN-OS integrated User-ID agent with server monitoring enabled
B) Standard Windows-based User-ID agent using WMI polling of domain controllers
C) Captive Portal configured with NTLM single sign-on authentication
D) Terminal Server (TS) agent configured with allocated source port ranges per user

---

**Q16.** An administrator configures a Destination NAT rule that translates traffic sent to the public address 203.0.113.10 into the internal server address 10.1.1.100, which resides in the DMZ zone. When creating the corresponding Security policy rule to permit this traffic, which destination address and destination zone should be configured?

A) Destination address 10.1.1.100 and destination zone Untrust
B) Destination address 10.1.1.100 and destination zone DMZ
C) Destination address 203.0.113.10 and destination zone DMZ
D) Destination address 203.0.113.10 and destination zone Untrust

---

**Q17.** A financial services company wants unknown executable files traversing the firewall to be uploaded to a cloud-based sandbox for dynamic behavioral analysis, receiving a verdict of malicious, grayware, phishing, or benign within minutes. Which Palo Alto Networks subscription provides this capability?

A) WildFire
B) DNS Security
C) URL Filtering
D) Vulnerability Protection

---

**Q18.** An administrator wants to identify and block DNS queries generated by malware using domain generation algorithms (DGA) to reach command-and-control infrastructure. Which subscription supplies the intelligence needed to dynamically categorize and block these domains?

A) Vulnerability Protection
B) DNS Security
C) Anti-Spyware
D) WildFire

---

**Q19.** By default, the predefined 'strict' Vulnerability Protection profile assigns which action to threat signatures with a severity of critical, high, or medium?

A) alert
B) default
C) reset-both
D) allow

---

**Q20.** A security engineer wants infected internal hosts that query known malicious domains to be redirected to a firewall-controlled IP address so the infected host can be easily identified in traffic logs. Which feature, configured within the Anti-Spyware profile, accomplishes this?

A) DNS Sinkhole
B) Vulnerability Protection exception
C) File Blocking profile
D) URL Filtering override

---

**Q21.** Which statement correctly differentiates the Antivirus security profile from WildFire analysis on a PAN-OS firewall?

A) The Antivirus profile submits unknown files to a cloud sandbox for dynamic analysis, while WildFire applies signature-based detection.
B) The Antivirus profile only inspects DNS traffic, while WildFire only inspects HTTP traffic.
C) Antivirus and WildFire perform identical functions and are interchangeable license names for the same feature.
D) The Antivirus profile blocks known malware inline using signatures (including signatures generated from WildFire verdicts), while WildFire performs sandbox analysis of unknown files to generate new signatures and verdicts.

---

**Q22.** A company wants to prevent employees from entering their corporate Active Directory credentials into websites categorized as 'newly-registered-domain' or 'unknown', while still allowing credential submission to internal SSO-approved sites. Which URL Filtering profile feature should the administrator configure?

A) Safe Search Enforcement
B) User Credential Detection with per-category submission actions
C) HTTP Header Insertion
D) Local override lists in PAN-DB

---

**Q23.** A security team enables DNS Security in their Anti-Spyware profile to protect against malware that communicates with command-and-control domains generated by DGA (Domain Generation Algorithms). They want infected internal hosts making malicious DNS queries to receive a fake IP address pointing to a Palo Alto Networks-controlled sinkhole, so the security team can identify which internal hosts are compromised by reviewing traffic logs for connections to that sinkhole address. Which DNS Security policy action should be configured for the relevant signature category to achieve this outcome?

A) Block
B) Alert
C) Allow
D) Sinkhole

---

**Q24.** A Panorama template stack contains three templates: 'Base-Network', 'Region-EU', and 'Site-London', listed in that order from top to bottom in the stack. Both 'Base-Network' and 'Site-London' configure the same NTP server address with different values. Which setting is pushed to the firewall assigned to this stack?

A) The setting from 'Base-Network' is used because it is highest in the stack
B) The setting from 'Site-London' is used because it is lowest in the stack
C) Panorama merges both values into a single comma-separated NTP configuration
D) The push fails with a validation error until the administrator manually resolves the conflict

---

**Q25.** Panorama manages a device group hierarchy where 'Branch-FW-01' is a child of the parent device group 'Global', and both device groups plus the Shared location contain pre-rulebase security rules. In which order does the firewall evaluate these pre-rules after Panorama pushes the configuration?

A) Branch-FW-01 pre-rules, then Global pre-rules, then Shared pre-rules
B) Shared pre-rules, then Branch-FW-01 pre-rules, then Global pre-rules
C) Shared pre-rules, then Global pre-rules, then Branch-FW-01 pre-rules
D) Global pre-rules, then Shared pre-rules, then Branch-FW-01 pre-rules

---

**Q26.** An enterprise manages 500 firewalls through Panorama and finds that log throughput is overwhelming a single Panorama management server used for both management and log collection. Which architecture change should be implemented to scale log ingestion without impacting management operations?

A) Increase the disk size on the existing Panorama management server virtual appliance
B) Deploy dedicated Log Collectors organized into one or more Collector Groups
C) Configure each firewall to forward logs directly to a third-party SIEM only, bypassing Panorama entirely
D) Enable log compression on each managed firewall to reduce the volume sent to Panorama

---

**Q27.** An administrator must standardize interface, zone, and NTP/DNS settings across 50 firewalls with varying network topologies, while keeping security policies and objects managed separately through device groups. Which Panorama construct should be used for the network and device-level settings?

A) Templates organized into template stacks
B) Device groups with shared objects
C) A single WildFire analysis profile applied globally
D) Dynamic address groups tagged per site

---

**Q28.** An administrator makes a security policy change in Panorama and selects Commit > Commit to Panorama, which completes successfully. The managed firewalls still enforce the old policy. What must the administrator do to apply the change to the firewalls?

A) Perform Commit > Push to Devices and select the relevant device groups and/or template stacks
B) Run Commit to Panorama a second time to force propagation
C) Log in to each firewall locally and perform a firewall-side commit
D) Reboot the Panorama management server to trigger automatic synchronization

---

**Q29.** An address object named 'WebServer-DC1' is created inside device group 'DC-Firewalls' (not in the Shared location). 'DC-Firewalls' has one child device group, 'DC-Firewalls-Branch'. Which firewalls can reference this object in policy pushed from Panorama?

A) Firewalls that are members of 'DC-Firewalls' or any of its child device groups, such as 'DC-Firewalls-Branch'
B) No firewalls, because device-group-level objects must first be promoted to Shared before any firewall can use them
C) Only firewalls that are direct members of 'DC-Firewalls', excluding any child device groups
D) All firewalls managed by Panorama, because Panorama objects are always globally visible

---

**Q30.** An administrator creates a template stack named STACK-DC that contains three templates in this order from top to bottom: TMPL-BASE, TMPL-REGION, TMPL-SITE. Both TMPL-BASE and TMPL-SITE configure a different value for the same DNS server setting under Device > Setup > Services. When Panorama pushes the template stack to the assigned firewall, which value is applied?

A) The value from TMPL-BASE, because the template with the highest priority in the stack (the topmost one) determines the value when settings overlap
B) The value from TMPL-SITE, because templates lower in the stack always override templates higher in the stack
C) Panorama merges both values into a single combined DNS configuration
D) The push fails with a validation error until the duplicate setting is removed from one of the templates

---

**Q31.** A network security engineer needs to upgrade a firewall running PAN-OS 10.1.6 to PAN-OS 11.1.2. Which upgrade path is correct?

A) Download PAN-OS 11.1.2 directly and install it, since PAN-OS supports direct upgrades across any number of feature releases.
B) Download and install the base image for 10.2.0, download and install the base image for 11.0.0, then download and install PAN-OS 11.1.2.
C) Download and install PAN-OS 11.1.2 directly, then reboot twice to complete the multi-release skip.
D) Downgrade to PAN-OS 9.1.0 first, then perform a fresh upgrade sequence to 11.1.2.

---

**Q32.** Two PA-5450 firewalls are deployed in an active/active HA pair to handle asymmetric traffic flows across two ISPs. Which mechanism ensures that return traffic for a session is delivered to the firewall that owns that session when asymmetric routing sends the packets to the wrong peer?

A) The HA3 packet-forwarding link forwards packets to the session owner firewall when asymmetric routing delivers them to the non-owner peer.
B) The HA2 data link synchronizes session tables between peers to keep both firewalls current.
C) The HA1 backup link carries heartbeat and control traffic as backup to the primary HA1 link.
D) A dedicated Layer 3 HA link using OSPF redistributes routes between the two peers.

---

**Q33.** A security team wants to build a report showing which GlobalProtect endpoints are missing required antivirus software, based on Host Information Profile compliance checks. Which PAN-OS log type should they use?

A) Traffic log
B) System log
C) HIP Match log
D) Authentication log

---

**Q34.** The GlobalProtect portal is configured to use a certificate issued by an internal enterprise CA. What must be done so that managed Windows endpoints connect to the portal without certificate trust warnings?

A) The portal certificate common name (CN) must match the firewall's management interface IP address.
B) The certificate must be signed by Palo Alto Networks' default self-signed CA.
C) The enterprise CA's root certificate must be distributed to the endpoints' trusted root certificate store.
D) The gateway must reuse the SSL decryption forward-trust certificate as the portal certificate.

---

**Q35.** What is the primary difference between saving a 'Named Configuration Snapshot' on a Palo Alto Networks firewall and exporting the 'Device State'?

A) A named configuration snapshot is stored only on Panorama, while device state is stored only locally on the firewall.
B) A named configuration snapshot only stores the configuration, while exporting device state also includes items such as certificates and other data needed to restore the device during an RMA.
C) A named configuration snapshot commits the configuration to the running config, while device state only saves it to the candidate config.
D) There is no meaningful difference; both produce identical files usable only during firmware upgrades.

---

**Q36.** In an active/passive HA pair, path monitoring on the active firewall detects failure of a path to a critical upstream router and triggers failover to the passive peer. Once the path issue is resolved, the network team wants to prevent the original firewall from automatically reclaiming the active role. Which setting should they configure?

A) Set the promotion hold timer to 0 seconds.
B) Disable preemption on both firewalls.
C) Increase the heartbeat interval to 10000 ms.
D) Enable 'Passive Link State' shutdown on the HA1 link.

---

**Q37.** An administrator needs to upgrade a firewall running PAN-OS 10.1.9 to the target release PAN-OS 11.1.4. The firewall has internet access to the Palo Alto Networks update servers. Which upgrade approach follows the correct procedure?

A) Update the content release version to the latest available, then download and install the 10.2.0 base image and reboot, then download and install the 11.0.0 base image and reboot, then download and install 11.1.4 and reboot
B) Directly download and install PAN-OS 11.1.4; the firewall will automatically retrieve any required intermediate base images during the install
C) Perform a factory reset and reinstall PAN-OS 11.1.4 from a USB recovery image to avoid version compatibility issues
D) Downgrade the firewall to PAN-OS 9.1.0 first, then perform a direct upgrade from 9.1.0 to 11.1.4 in a single step

---

**Q38.** A company wants only its corporate-managed laptops to be able to establish a GlobalProtect VPN connection to the gateway, even if an employee's valid username and password are entered from a personal, unmanaged device. Which configuration should the engineer implement on the GlobalProtect gateway?

A) Enable a Captive Portal identification profile on the gateway to redirect unauthenticated users to a web login page
B) Configure SAML single sign-on with an external identity provider as the sole authentication method
C) Enable a pre-logon connect method using only LDAP directory authentication for the gateway
D) Require client (machine) certificate authentication using a certificate profile tied to a certificate issued only to managed devices, in addition to user authentication

---

**Q39.** A company runs a Palo Alto Networks active/active high availability pair as the internet edge firewall performing source NAT on outbound traffic. They require that the source NAT IP address remain identical no matter which firewall in the pair processes a given session, so that stateful upstream devices do not see the translated address change during a failover event. What should the engineer configure?

A) Two separate static NAT address pools, one per firewall, selected based on each device's HA device-ID
B) A dedicated floating IP address, shared by the HA pair, configured as the NAT source address
C) A DIPP (dynamic IP and port) NAT pool bound individually to each firewall's dataplane interface
D) A policy-based forwarding rule that forces all outbound sessions through the active-primary firewall only

---

**Q40.** A firewall administrator is troubleshooting a connectivity issue. The Traffic log shows the session end reason as 'policy-deny', yet the administrator confirms a Security policy rule permitting this traffic exists near the top of the rulebase, above the deny-all rule. What is the MOST likely explanation?

A) The candidate configuration containing the allow rule was never committed
B) A Zone Protection profile on the ingress zone is silently dropping the session
C) The firewall is operating in virtual wire mode, which ignores Security policy zone matching
D) The session was initially allowed based on port, but after App-ID identified the true application, the firewall re-evaluated the session against the Security policy and matched a different rule with a deny action

---

**Q41.** An administrator wants to identify which stage of the single-pass packet processing pipeline is dropping packets, using global drop counters rather than a full packet capture. Which CLI command should be used?

A) show session all filter application any
B) request session-cleanup
C) show high-availability all
D) show counter global filter delta yes

---

**Q42.** An administrator enables SSL Forward Proxy decryption. Almost immediately, users report browser certificate warnings on nearly all HTTPS sites they visit. What configuration step resolves this issue?

A) Increase the SSL decryption port range in the Decryption profile
B) Enable decrypt mirroring on the decryption policy
C) Disable SSL/TLS protocol version checking in the Decryption profile
D) Distribute and install the firewall's Forward Trust CA certificate into each client's trusted root certificate store

---

**Q43.** Remote users can successfully authenticate at the GlobalProtect portal, but the connection to the GlobalProtect gateway fails with a certificate trust error in the client logs. What is the MOST likely cause?

A) The gateway's SSL/TLS service profile references a certificate that is not trusted by the client, such as a self-signed certificate without the issuing CA installed
B) The GlobalProtect gateway license has expired
C) The client configuration's IP address pool overlaps with the internal network
D) The portal agent configuration is missing an authentication profile

---

**Q44.** In an active/passive HA pair, both firewalls unexpectedly transition to the active state simultaneously, resulting in a split-brain condition. Investigation reveals that only a single HA1 control link exists between the peers. What is the MOST likely root cause?

A) Session synchronization over HA2 was disabled
B) No HA1 backup link was configured, so the single control link failure prevented the peers from exchanging heartbeats and each assumed the other had failed
C) Path monitoring was disabled on both firewalls
D) The HA group ID was set to the same value on both firewalls

---

**Q45.** An administrator managed by Panorama attempts to commit a firewall's local configuration change but receives a 'commit lock' error preventing the commit from proceeding. What is the cause of this condition?

A) Another administrator currently holds an active configuration lock on the candidate configuration, which must be released before others can commit
B) The device group contains an invalid Security rule that fails validation
C) The template stack referenced by the firewall was deleted from Panorama
D) Commit lock is a default, non-configurable behavior that always requires a reboot to clear

---

**Q46.** A network security engineer is troubleshooting a site-to-site IPSec VPN tunnel between a Palo Alto Networks firewall and a third-party VPN gateway. The 'show vpn ike-sa' output confirms the IKE Phase 1 SA is in the 'I2' state and considered up. However, 'show vpn ipsec-sa' shows no active Phase 2 SA, and the tunnel interface remains down. The System log shows repeated 'IPSec Phase-2 SA proposal not accepted' messages. What is the MOST likely cause of this issue?

A) The pre-shared key configured on the local firewall does not match the key configured on the remote peer
B) The proxy-id (local/remote subnet) or encryption/authentication settings defined in the IPSec Crypto profile do not match between the two peers
C) The tunnel interface has not been assigned to a security zone, causing the firewall to drop the negotiation
D) The Dead Peer Detection (DPD) interval configured on the firewall is shorter than the interval configured on the remote peer

---

