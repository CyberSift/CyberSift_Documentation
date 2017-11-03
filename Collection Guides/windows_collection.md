# Collecting and Analysing Windows Event logs

## Table Of Contents
* [Overview](https://github.com/CyberSift/CyberSift_Documentation/blob/master/Collection%20Guides/ossec_collection.md#overview)
* [Procedure](https://github.com/CyberSift/CyberSift_Documentation/blob/master/Collection%20Guides/windows_collection.md#procedure)
* [Inserting Tenant Information](https://github.com/CyberSift/CyberSift_Documentation/blob/master/Collection%20Guides/ossec_collection.md#inserting-tenant-information)
    
## Overview

The ELK infrastructure makes it very easy to collect logs from windows servers. The CyberSift Windows Module then uses the collected information as input to several machine learning algorithms to highlight abnormalities in the logs. Currently the supported log sources are:

- Windows Security Logs: Highlight abnormal user logins from different IPs, Workstations, or Processes

![screenshot](https://github.com/CyberSift/CyberSift_Documentation/raw/master/Collection%20Guides/static/img/IMG-20171102-WA0003.jpg "CyberSift Nebula showing anomalous user login")

- Windows System Logs: Highlight abnormal log entries and sequences

![screenshot](https://github.com/CyberSift/CyberSift_Documentation/raw/master/Collection%20Guides/static/img/system_log_event_timeline.png "CyberSift Nebula showing anomalous system log events)

## Procedure

- [Install winlogbeat as per instructions](https://www.elastic.co/guide/en/beats/winlogbeat/current/winlogbeat-installation.html)
- When configuring winlogbeat, **disable** any elasticsearch outputs and instead **enable** logstash outputs, for example:

```
output.logstash:
    hosts: ["192.168.1.1:5044"]
```
where "192.168.1.1" should be replaced with your CyberSift IP address

## Inserting Tenant Information

If you are using multi-tenancy, we have a couple more steps of configuration:
- In the CyberSift UI Dashboard, create a new tenant from "Configuration > Manage Tenants", making a note of the tenant name chosen
![creating a new tenant](https://github.com/CyberSift/CyberSift_Documentation/raw/master/Collection%20Guides/static/img/create_tenant.png)

- In the winlogbeat configuration, add the following configuration to the **winlogbeat.yml** file:

```
fields_under_root: true
fields:
    tenant: "testlab"
```

replacing "testlab" with the name of the tenant chosen in the previous step

# Notes
- Tested using winlogbeat v5.6.3, [available here](https://www.elastic.co/downloads/past-releases/winlogbeat-5-6-3)
- Full winlogbeat configuration options [available here](https://www.elastic.co/guide/en/beats/winlogbeat/current/configuration-winlogbeat-options.html)
