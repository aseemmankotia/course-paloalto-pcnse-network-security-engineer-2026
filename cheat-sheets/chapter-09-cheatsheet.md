# Chapter 9 Cheat Sheet: GlobalProtect, Certificates, Logging

## GlobalProtect Role Separation
| Component | Function |
|---|---|
| **Portal** | Distributes app, client config, gateway list. No tunnel termination. |
| **Gateway** | Terminates VPN tunnel, authenticates users, enforces HIP-based policy. |

**Exam trap:** Reversed portal/gateway roles are the most common wrong answer pattern.

## Split Tunnel vs Full Tunnel
- Split tunnel: Access Route list contains only specific internal subnets (e.g., `10.0.0.0/8`).
- Adding `0.0.0.0/0` to Access Route = full tunnel behavior, even if split was intended.
- "No direct access to local network" checkbox forces ALL traffic (including local LAN) into tunnel.

## HIP (Host Information Profile)
- **HIP Object**: single criterion (disk encryption, patch mgmt, AV, firewall state, etc.)
- **HIP Profile**: boolean combination of HIP objects.
- **Enforcement point**: HIP Profile referenced in a **Security Policy rule** on the **gateway**. Portal never enforces HIP.

## Certificate Hierarchy
| Type | Purpose |
|---|---|
| Root CA | Self-signed, top of trust chain; must be marked "Trust for..." |
| Intermediate CA | Signs end-entity certs; chains up to root |
| Device Certificate | Firewall identity (mgmt, Panorama, GP gateway auth) |
| SSL/TLS Service Certificate | Bound to a Service Profile for HTTPS-facing features (portal, gateway, mgmt UI, Captive Portal) |
| Forward Trust CA | Internal CA firewall uses to sign dynamically generated certs for SSL Forward Proxy decryption |

## Certificate Profile Key Settings
- **CRL / OCSP**: revocation checking methods (can configure both).
- **Block Session If Certificate Status Is Unknown**: unchecked = allow session when revocation responder unreachable; checked = block. Classic exam gotcha.
- **Username Field**: Subject CN or Subject Alt Name — must match how certs were issued for cert-based auth to extract the correct username.

## Log Forwarding
- **Log Forwarding Profile** must be attached to a **Security Policy rule** (Actions tab) to forward traffic/threat/WildFire logs — the profile does nothing unattached.
- Destinations: **Syslog, Email, SNMP trap, Panorama**.
- SNMP trap / Email = best for lightweight alerting, NOT bulk SIEM ingestion.
- Syslog + Panorama simultaneously = standard pattern for SIEM integration + compliance retention.
- System/Config logs use default log settings (Device tab), separate from policy-based log forwarding profiles.

## Quick Command/UI Reference
- Portal config: `Network > GlobalProtect > Portals`
- Gateway config: `Network > GlobalProtect > Gateways`
- HIP Objects/Profiles: `Objects > GlobalProtect > HIP Objects / HIP Profiles`
- Certificate Profiles: `Device > Certificate Management > Certificate Profile`
- Log Forwarding Profiles: `Objects > Log Forwarding`, attached under Policy rule > Actions tab
