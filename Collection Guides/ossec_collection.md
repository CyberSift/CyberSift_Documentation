# Collecting and Analysing OSSEC logs

## Table Of Contents
* [Overview](https://github.com/CyberSift/CyberSift_Documentation/blob/master/Collection%20Guides/ossec_collection.md#overview)
* [Procedure](https://github.com/CyberSift/CyberSift_Documentation/blob/master/Collection%20Guides/ossec_collection.md#procedure)
* [Analysing OSSEC logs in CyberSift](https://github.com/CyberSift/CyberSift_Documentation/blob/master/Collection%20Guides/ossec_collection.md#analysing-ossec-logs-in-cybersift)
 * [Context Enrichment](https://github.com/CyberSift/CyberSift_Documentation/blob/master/Collection%20Guides/ossec_collection.md#context-enrichment)
 * [Hyper-Alerting](https://github.com/CyberSift/CyberSift_Documentation/blob/master/Collection%20Guides/ossec_collection.md#hyperalerting)

## Overview

[OSSEC](http://ossec.github.io/) is a popular rule based HIDS that can be deployed on both windows and linux. CyberSift's ElasticSearch backend can be used to collect and visualize the logs that an OSSEC deployment generates. Once the logs are stored within CyberSift, you can define alert rules that trigger on a variety of conditions such as sudden increases, flatlines, or alert on keywords

In addition, CyberSift applies machine learning techiques to perform *hyper-alerting*, alerting on abnormalities such as:
* rarely seen OSSEC events that may warrant further investigation
* abnormal combinations of events that are not usually seen in your environment

**Hyper-Alerting** helps your analysts focus on deviations from the norm - those events that are probably of more significance to your security stance.

## Procedure

1. Install OSSEC in your environment.
There are several guides available on how to do this:
  * [UNIX](https://github.com/ossec/ossec-hids)
  * [CentOS 7](https://www.vultr.com/docs/how-to-install-ossec-hids-on-a-centos-7-server)
  * [Windows Agents](http://ossec.github.io/downloads.html)
  
**TIP:** always use the latest version to avoid bugs. The easiest is to install your OSSEC server on a linux box using the procedure outlined in [OSSEC's github page](https://github.com/ossec/ossec-hids). Linux agents can be installed using the same procedure, while Windows agent binaries can be downloaded from the link above.

2. In preperation for shipping logs to CyberSift, [configure OSSEC to output logs in JSON format](http://ossec-docs.readthedocs.io/en/latest/manual/output/json-alert-log-output.html#enabling-json-output). In a typical installation, this will output JSON formatted logs to **/var/ossec/logs/alerts/alerts.json**

**TIP:** if ossec complains that the *jsonout_output* configuration directive is not recognised, you need to upgrade to the latest version

3. On the OSSEC server, install [Elastic Filebeat](https://www.elastic.co/products/beats/filebeat). This agent will be responsible for shipping logs off to CyberSift. 

4. Once installed, configure filebeat to read OSSEC logs and send them to CyberSift. Below is a sample configuration file that will do this

```
filebeat:
  prospectors:
    -
      paths:
                - /var/ossec/logs/alerts/alerts.json
      input_type: log
      ignore_older: 168h
      close_inactive: 24h
      document_type: ossec_log
      json.keys_under_root: true  

output.elasticsearch:
  hosts: ["http://192.168.1.1:80/cybersift_elasticsearch"]
  template.enabled: true
  template.path: "filebeat.template.json"
  template.overwrite: false
  index: "ossec-%{+yyyy.MM.dd}" 
```

**NOTE 1:** Make sure to change the IP address as appropriate to your environment but it's important to keep the ***:80/cybersift_elasticsearch*** part of the URL

**NOTE 2:** Spacing & indentation is important in YAML configuration files like the one above, so if you have problems do a double take on this...

5. At this stage, OSSEC is shipping logs to CyberSift. We need to instruct Kibana to start displaying the collected OSSEC information. 
This is done by simply adding a new index. From the CyberSift Dashboard, click on the **CyberSift Logging Engine** option, click on **Management > Index Patterns** and type in **ossec-\*** as shown below.

![Defining the ossec index in CyberSift/Kibana](https://docs.google.com/drawings/d/1ieNOkhT6g6wFKp8A7HtsyaMnRg4z8_mEw7xEuw6DLEA/pub?w=596&h=544)

This allows us to start visualizing the logs as shown below, and creating dashboards using these logs

![OSSEC logs in CyberSift/Kibana](https://docs.google.com/drawings/d/13kHPKOayCxIrWfqcOYIHpUKXtleAnlntlc1xuQS6GFw/pub?w=941&h=329)


## Analysing OSSEC logs in CyberSift

### Context Enrichment

TODO

### HyperAlerting

TODO
