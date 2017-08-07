# CyberSift IP module

## Table Of Contents
- [Overview](https://github.com/CyberSift/CyberSift_Documentation/tree/master/CyberSift%20Modules/IP#overview)
- [Why Use the IP Module?](https://github.com/CyberSift/CyberSift_Documentation/tree/master/CyberSift%20Modules/DNS#why-use-the-ip-module)

## Overview

TODO

## Why Use the IP Module

TODO

## Writing User Defined Templates

In order to ingest data and parse it properly, CyberSift requires a template to be defined. CyberSift data collection engine is powered by ElasticSearch and LogStash, making it easy to define your own template. 

As a practical example, consider the following firewall template for Sonicwall firewalls:

```
filter {
  if [type] == "syslog" and ([message] =~ /Connection Closed/ or [message] =~ /proto=tcp\/http/) and [source] =~ /^\/var\/log\/remotesyslog\/192.168.0.0.*/ {

        kv {
                exclude_keys => [ "c", "id", "m", "n", "pri" ]
        }
        grok {
                match => [ "src", "%{IP:SourceAddress}:%{INT:SourcePort}:%{WORD:InboundInterface}" ]
        }
        grok {
                match => [ "dst", "%{IP:DestinationAddress}:%{INT:DestinationPort}:%{WORD:OutboundInterface}" ]
        }
        grok {
                match => [ "proto", "%{WORD:IPprotocol}/%{USERNAME:proto_info}" ]
        }


	date {
             match => [ "time", "YYYY-MM-dd HH:mm:ss" ]
        }

	geoip {
    	  source => "DestinationAddress"
	      target => "dst_geoip"
	}


	geoip {
          source => "SourceAddress"
          target => "src_geoip"
        }


	mutate {
	    add_tag => ["Sonicwall_Traffic"]
	}

	mutate {
	    add_field => { "SourceDestinationTuple" => "%{SourceAddress}:%{DestinationAddress}:%{DestinationPort}" }
	}

  mutate {
	    add_field => { "Type" => "TRAFFIC" }
	}


    syslog_pri { }

    mutate {
        rename => { "sent" => "BytesSent" }
        rename => { "rcvd" => "BytesReceived" }
    }

    mutate {
		add_field => { "received_at" => "%{@timestamp}" }
        convert => [ "BytesReceived", "integer" ]
        convert => [ "BytesSent", "integer" ]
    }

    ruby {
        code => "event.set('Bytes',event.get('BytesReceived') + event.get('BytesSent'))"
    }

  }

  if [type] == "syslog" and [message] =~ /Connection Opened/ and [source] =~ /^\/var\/log\/remotesyslog\/192.168.0.0.*/ {
    drop {}
  }

}
```

There are several important points here:

- There are a number of fields which are compulsory. If the following field names are not present in the document, subsequent processing of these documents will fail (**note that field names are CaSe SeNsItIvE**):

| Field Name | Type | Description |
| :--------: | :--: | :---------: |
| @timestamp | date | The timestamp of the log entry |
| Type | string | This must be set to **TRAFFIC**, indicating to CyberSift that this is IP traffic |
| SourceAddress | string | The Source IP Address of the log entry |
| DestinationAddress | string | The Destination IP Address of the log entry |
| DestinationPort | integer | The Destination Port of the log entry |
| Bytes | integer | The amount of bytes transferred for this connection / log entry |
| IPprotocol | string | Must be set to **tcp** or **udp** |
| SourceDestinationTuple | string | A tuple in the form %{SourceAddress}:%{DestinationAddress}:%{DestinationPort}, eg *192.168.0.1:8.8.8.8:53* |

In the above example, note the use of **mutate** the rename the original fields to match those presented in the table above. Also note how we use **mutate** to modify the field type to match those described in the table. 

- Note how **mutate** is used to populate missing fields (such as Type = TRAFFIC) 
- Note how **date** is used to parse a date field which is later used in a mutate to populate the required **@timestamp** field

- Each entry will automatically be given the field **type** equal to **syslog**. Do not change this, it is used for filtering. In fact, as per the above example, every filter should start with a line similar to:

```
if [type] == "syslog" and [source] =~ /^\/var\/log\/remotesyslog\/192.168.0.0.*/
```
Change the IP address above (192.168.0.0) as appropriate - it should be equal to the IP address of your given source. If the above line is not present, the filter will apply to all input - which is very probably not what you want.

- In the above example, note how the **Bytes** field is actually the addition of the original **BytesSent** and **BytesReceived** fields, performed in the **ruby** section. This is useful in case you do not have a single Bytes field as per this example

- You will probably make heavy use of **grok**, but there are a lot of guides on how to write GROK expressions online. A very useful tool is the GROK debugger:

[Grok Debugger](https://grokdebug.herokuapp.com/)

 
