# Sending Connection Logs to CyberSift without a firewall or syslog

Should your scenario require you not to use a firewall, syslog or netflow traffic, it is still possible to send network data to CyberSift. This can be done via [port mirroring](https://www.miarec.com/faq/what-is-port-mirroring) and sending a copy of network traffic to a network sniffer. 

### Port Mirroring

![mirroring animation](https://raw.githubusercontent.com/CyberSift/CyberSift_Documentation/master/Collection%20Guides/static/img/port_mirror.gif)

A port mirror essentially sends copies of your network data to a designated "Analysis Port". This allows you to monitor your network traffic without any changes to your infrastructure, and without requiring any firewalls, syslog, or netflow. Almost all vendors support port mirroring in one form or another, including networking vendors such as Cisco, Dell and many more.

### The Network Sniffer

![ntopng logo](https://raw.githubusercontent.com/CyberSift/CyberSift_Documentation/master/Collection%20Guides/static/img/Screenshot%20from%202017-12-19%2011-56-18.png)

The next stage is to deploy a network sniffer attached to the analysis port. CyberSift supports the excellent [**NTOPNG**](https://www.ntop.org/products/traffic-analysis/ntop/)  which comes in both commercial and open-source variants, maing it perfect for both enterprises and low-budget deployments. Installing ntopng is extremely easy. For example on a Ubuntu server simply issue the following commands as root:


```
apt-get install redis-server ntopng
ntopng -F "logstash;W.X.Y.Z;tcp;5510" #replace W.X.Y.Z with your CyberSift server IP
```

The above command will start ntopng sniffing all traffic and sending it to CyberSift - which will take care of the rest! 
*(Should you require further help and advice, CyberSift support will be happy to help)*


### CyberSift

There isn't any additional configuration necessary on CyberSift. Once NTOPNG is correctly setup, CyberSift will begin collecting data, which can be viewed via the "CyberSift Logging Engine" in the **filebeat** index. 

One additional advantage of deploying CyberSift in this manner is that by leveraging NTOP we obtain Layer 7 visibility - you will be able to view the traffic traversing your network at an application level, regardless of what port that traffic uses, as can be seen in the screenshot below

![L7 Visibility](https://raw.githubusercontent.com/CyberSift/CyberSift_Documentation/master/Collection%20Guides/static/img/CyberSift%20L7%20Protocol.jpeg)

This functionality is possble when gathering data via firewall syslogs, but it depends on the firewall capabilities (e.g. Palo Alto Network and SonicWall firewalls support exporting layer 7 application data via syslog - please check with your firewall vendor for additional details)
