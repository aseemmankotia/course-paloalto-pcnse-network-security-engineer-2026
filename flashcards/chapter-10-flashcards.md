## Chapter 10: Structured Troubleshooting: Packet Flow, Logs, and Packet Captures — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | PAN-OS packet capture stage that captures packets on ingress BEFORE any policy processing? | receive (rx). Exam tip: order is receive → firewall → transmit, with drop capturing discards at any point. |
| 2 | PAN-OS packet capture stage that captures packets AFTER security/NAT/content processing but before egress? | firewall stage (post security processing). Exam tip: use this stage to confirm a packet passed policy lookup before checking egress. |
| 3 | CLI command to simulate a flow against the security rulebase without sending traffic? | test security-policy-match source <ip> destination <ip> destination-port <port> protocol <num> from <zone> to <zone>. Exam tip: returns the exact rule name that would match. |
| 4 | CLI command to simulate a flow against the NAT rulebase? | test nat-policy-match from <zone> to <zone> source <ip> destination <ip> destination-port <port> protocol <num>. Exam tip: distinct from test security-policy-match — NAT rules only. |
| 5 | CLI command to check URL category classification from PAN-DB cloud? | test url-info-cloud <url>. Local cache variant: test url-info-host <url>. Exam tip: cloud = authoritative live lookup, host = local cache only. |
| 6 | Command to view details of one specific existing session (matched rule, NAT, app, flags)? | show session id <session-id>. Exam tip: use 'show session all filter' to search the session table for a matching flow first, then drill into the id. |
| 7 | Traffic log session end reason meaning a security rule with deny/drop action ended the session? | policy-deny. Exam tip: distinguish from tcp-rst-from-client/server (endpoint-driven) and aged-out (idle timeout). |
| 8 | Session end reason indicating a decryption session failed due to a bad/untrusted/expired server certificate? | decrypt-cert-validation. Exam tip: caused by the Decryption Profile's 'Block sessions with expired/untrusted/unsupported cert' settings. |
| 9 | HA link that carries heartbeats and state synchronization info between HA peers? | HA1 (control link). Exam tip: HA1 failure while HA2 stays up is the classic split-brain trigger in active/passive HA. |
| 10 | HA link that synchronizes session table and configuration data (not heartbeats)? | HA2 (data link). Exam tip: HA2 loss alone does not cause split-brain; HA1 loss does. |
| 11 | What HA scenario occurs when both peers in active/passive believe they are active simultaneously? | Split-brain — typically caused by HA1 control link failure preventing heartbeat exchange. Exam tip: first troubleshooting step is verifying HA1 connectivity/config. |
| 12 | Which HA link is used only in active/active deployments for forwarding packets between peers for session ownership reasons? | HA3 (packet forwarding link). Exam tip: HA3 is irrelevant to active/passive troubleshooting — don't confuse it with HA1/HA2 failures. |
