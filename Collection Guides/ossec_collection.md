# Collecting and Analysing OSSEC logs

## Overview

[OSSEC](http://ossec.github.io/) is a popular rule based HIDS that can be deployed on both windows and linux. CyberSift's ElasticSearch backend can be used to collect and visualize the logs that an OSSEC deployment generates. Once the logs are stored within CyberSift, you can define alert rules that trigger on a variety of conditions such as sudden increases, flatlines, or alert on keywords

In addition, CyberSift applies machine learning techiques to perform *hyper-alerting*, alerting on abnormalities such as:
* rarely seen OSSEC events that may warrant further investigation
* abnormal combinations of events that are not usually seen in your environment

Hyper-Alerting helps your analysts focus on deviations from the norm - those events that are probably of more significance to your security stance.

## Procedure

TODO

![Defining the ossec index in CyberSift/Kibana](https://docs.google.com/drawings/d/1ieNOkhT6g6wFKp8A7HtsyaMnRg4z8_mEw7xEuw6DLEA/pub?w=596&h=544)

![OSSEC logs in CyberSift/Kibana]
(https://docs.google.com/drawings/d/13kHPKOayCxIrWfqcOYIHpUKXtleAnlntlc1xuQS6GFw/pub?w=941&h=329)


## Analysis

TODO
