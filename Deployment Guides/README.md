# Deployment Guides

CyberSift can be deployed in a number of ways, depending on your security and log collection needs. If you'd like further information on what you should collect please refer to the [Security Sources Collection Guide Overview](#).Below we outline the most popular methods to deploy CyberSift 

### Overview

CyberSift can be deployed in one of two ways: 

  - #### On premise
A Virtual Machine can be provided (VMWare / VirtualBox... other vendor support coming asap), or alternatively you can buy a physical server from CyberSift. The advantages of this approach are response times, complete privacy and control. Disadvantages include implementation and maintanance costs.    
  
  - #### Cloud based
CyberSift provides AWS-backed cloud services which allow for provisioning and running the system in the cloud. This approach has many advantages, including not having to worry about operation & maintenance. However, you must ensure that your monitored network has sufficient upload bandwidth to support shipping data to the cloud. This tends to be the more economical approach especially for smaller environments

### What Would You Like To Do?

- I would like to send my firewall connection logs to CyberSift
- I would like to use CyberSift Endpoint to enrich the logs sent to CyberSift
- I would like to use CyberSift to monitor Windows Event Logs (beta)
- I would like to use CyberSift to monitor my HTTP Webservers
- I would like to use CyberSift to monitor my DNS requests
- I would like to use Snort to enrich the logs sent to CyberSift
- I would like to use OpenVas to enrich the logs sent to CyberSift
