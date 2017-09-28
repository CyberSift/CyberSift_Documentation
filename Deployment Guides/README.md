# Deployment Guides

CyberSift can be deployed in a number of ways, depending on your security and log collection needs. If you'd like further information on what you should collect please refer to the [Security Sources Collection Guide Overview](#). Below we outline the most popular methods to deploy CyberSift 

### Overview

CyberSift can be deployed in one of two ways: 

  - #### On premise
A Virtual Machine can be provided (VMWare / VirtualBox... other vendor support coming asap), or alternatively you can buy a physical server from CyberSift. The advantages of this approach are response times, complete privacy and control. Disadvantages include implementation and maintanance costs.    
  
  - #### Cloud based
CyberSift provides AWS-backed cloud services which allow for provisioning and running the system in the cloud. This approach has many advantages, including not having to worry about operation & maintenance. However, you must ensure that your monitored network has sufficient upload bandwidth to support shipping data to the cloud. This tends to be the more economical approach especially for smaller environments

### What Would You Like To Do?

- [I would like to deploy CyberSift in the AWS cloud environment](https://github.com/CyberSift/CyberSift_Documentation/blob/master/Deployment%20Guides/cybersift-aws.md)

Deploy CyberSift in the AWS cloud

- [I would like to send my firewall's syslog connection logs to CyberSift](https://github.com/CyberSift/CyberSift_Documentation/blob/master/Deployment%20Guides/firewall-connections-syslog.md)
- [I would like to send my netflow connection logs to CyberSift](https://github.com/CyberSift/CyberSift_Documentation/blob/master/Deployment%20Guides/connections-netflow.md)

These would normally be the first steps in any deployment, and forms the basis for CyberSift's IP Module. [What if I don't have a central firewall that supports syslog or neflow?](https://github.com/CyberSift/CyberSift_Documentation/blob/master/Deployment%20Guides/connections-logs-without-firewall.md)

- [I would like to use CyberSift Endpoint to enrich the logs sent to CyberSift](https://github.com/CyberSift/CyberSift_Documentation/blob/master/Deployment%20Guides/cybersift-endpoint-network-processes.md)

CyberSift also allows you to vizualize which processes (e.g Skype, Chrome, Apache, ma1wAr3.exe) handled the connection that you see on the console. This is currently supported on Linux and Windows systems only.

- [I would like to use CyberSift to monitor Windows Event Logs (beta)](#)



- [I would like to use CyberSift to monitor my HTTP Webservers](#)



- [I would like to use CyberSift to monitor my DNS requests](https://github.com/CyberSift/CyberSift_Documentation/tree/master/CyberSift%20Modules/DNS)

CyberSift monitors and performs anomaly detection on your environment's DNS traffic, allowing you to mitigate a wide of threats.

- [I would like to use Snort to enrich the logs sent to CyberSift](#)



- [I would like to use OpenVas to enrich the logs sent to CyberSift](#)

### Further Details
- [What are the inbound and outbound ports that the CyberSift server requires?](https://github.com/CyberSift/CyberSift_Documentation/blob/master/Deployment%20Guides/inbound-outbound-ports.md)
