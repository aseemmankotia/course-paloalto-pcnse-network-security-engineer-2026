# Chapter 4 Lab: Content-ID and SSL/TLS Decryption

**Estimated time:** 15 minutes

## Exercise

Read-and-understand walkthrough: compare two decryption policy scenarios (outbound web browsing vs inbound access to an internal web server) and identify which decryption mode and certificate each requires

## Key takeaway

Choosing the correct decryption mode depends on traffic direction: Forward Proxy for outbound client traffic using a trusted CA cert, Inbound Inspection for traffic to internal servers using the server's own certificate and key.
