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



## Inserting Tenant Information


# Notes
- Tested using winlogbeat v5.6.3, [available here](https://www.elastic.co/downloads/past-releases/winlogbeat-5-6-3)
- Full winlogbeat configuration options [available here](https://www.elastic.co/guide/en/beats/winlogbeat/current/configuration-winlogbeat-options.html)
