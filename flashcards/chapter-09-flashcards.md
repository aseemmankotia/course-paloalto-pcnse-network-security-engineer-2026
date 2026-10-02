## Chapter 9: GlobalProtect, Certificate Management, and Logging — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | GlobalProtect Portal — core function? | Distributes the GlobalProtect app, client configuration, and gateway list to endpoints. Does NOT terminate VPN tunnels. Exam tip: portal = config delivery only. |
| 2 | GlobalProtect Gateway — core function? | Terminates the actual VPN tunnel, authenticates users, and enforces HIP-based security policy. Exam tip: HIP enforcement happens at the gateway, never the portal. |
| 3 | Split tunnel Access Route contains 0.0.0.0/0 — effect? | Forces full-tunnel behavior even though split tunnel was intended, since the default route pulls all traffic into the tunnel. Common exam trap. |
| 4 | Where is HIP-based access enforced? | Via a HIP profile (built from HIP objects like disk encryption, patch management) referenced in a Security Policy rule on the gateway — not configured at the portal. |
| 5 | Certificate chain hierarchy order? | Root CA (self-signed, top) → Intermediate CA (signs end-entity certs) → Device/Server certificate (identity of firewall/server). Root CA must be marked 'Trust for...' to validate the chain. |
| 6 | Certificate Profile: 'Block Session If Certificate Status Is Unknown' | If checked, blocks sessions when OCSP/CRL status can't be determined (e.g., responder unreachable). Unchecking allows the session while still enforcing checks when status IS known. Frequent exam gotcha. |
| 7 | SSL/TLS Service Profile — purpose? | Binds a server certificate plus min/max TLS protocol version to a firewall-facing service (GlobalProtect portal/gateway, management UI, Captive Portal). Required for HTTPS access to those services. |
| 8 | Requirement to avoid browser warnings during SSL Forward Proxy decryption? | Import an enterprise/internal CA as the Forward Trust CA on the firewall (it signs dynamically generated certs), and push that CA's chain to endpoint trust stores via GPO/MDM. |
| 9 | How does a Log Forwarding Profile take effect? | It must be attached to the Actions tab of one or more Security Policy rules (or default log settings for system/config logs). The profile alone does nothing unattached. |
| 10 | Supported Log Forwarding destinations? | Syslog, Email, SNMP trap, and Panorama. Panorama can also receive logs independently via its own Collector Group forwarding configuration, separate from firewall log forwarding profiles. |
| 11 | Device certificate vs. SSL/TLS Service certificate — difference? | Device certificate = firewall's identity for management/GlobalProtect authentication with Panorama or GP client. SSL/TLS Service certificate = bound in a Service Profile for a specific HTTPS-facing feature (portal, gateway, mgmt UI). |
| 12 | Certificate Profile 'Username Field' setting — purpose? | Specifies which certificate field (Subject CN or Subject Alt Name) is used to extract the username for client-certificate-based GlobalProtect authentication. Exam tests matching this to how certs were actually issued. |
