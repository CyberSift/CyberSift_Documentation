# What are the inbound and outbound ports that the CyberSift server requires?

### Inbound Ports Required:

- Support management port (please contact CyberSift support)
- If using syslog: UDP port 514
- If using netflow: UDP port 2055
- If using beats: TCP port 9200

### Outbound Ports Required:

- UDP Port 53 (DNS Lookups)
- TCP Port 80/443 (CyberSift Cerebrum)
- TCP Port 9202 (CyberSift Licensing)
