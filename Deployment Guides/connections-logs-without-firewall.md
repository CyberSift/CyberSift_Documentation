# Sending Connection Logs to CyberSift without a firewall or syslog

Should your scenario require you not to use a firewall, syslog or netflow traffic, it is still possible to send network data to CyberSift. This can be done via [port mirroring](https://www.miarec.com/faq/what-is-port-mirroring) and sending a copy of network traffic to a network sniffer. 

### Port Mirroring

![mirroring animation](https://raw.githubusercontent.com/CyberSift/CyberSift_Documentation/master/Collection%20Guides/static/img/port_mirror.gif)

A port mirror essentially sends copies of your network data to a designated "Analysis Port". This allows you to monitor your network traffic without any changes to your infrastructure, and without requiring any firewalls, syslog, or netflow. Almost all vendors support port mirroring in one form or another, including networking vendors such as Cisco, Dell and many more.

### The Network Sniffer

### CyberSift

![L7 Visibility](https://raw.githubusercontent.com/CyberSift/CyberSift_Documentation/master/Collection%20Guides/static/img/CyberSift%20L7%20Protocol.jpeg)
