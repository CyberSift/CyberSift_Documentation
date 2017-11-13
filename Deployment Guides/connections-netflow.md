# Collecting Netflow Records

The CyberSift server also acts as a Netflow collector, meaning that you can point your netflow probes towards the CyberSift server **on UDP port 2055**. Most firewalls/routers support netflow - alternatively look to using programs such as [FProbe](http://fprobe.sourceforge.net/) or [NProbe](http://www.ntop.org/products/netflow/nprobe/)). The CyberSift server supports Netflow versions 5,9 and 10 (IPFIX) out of the box.

## Building your own netflow collector

In some situations (such as pre-existing netflow setup, or a distributed CyberSift setup) it may be necessary to decouple the netflow server from the CyberSift processing server. This is a straightforward procedure and is mostly based around Elastic's [Logstash](https://www.elastic.co/products/logstash). Simply follow the instructions to install logstash on your preferred platform, and use the following logstash configuration file:

```
input {
  udp {
    host => "0.0.0.0"
    port => 2055
    codec => netflow {
        versions => [5, 9, 10]
    }
    type => netflow
    add_field => {
        "[@metadata][beat]" => "filebeat"
        "[@metadata][type]" => "netflow"
    }
  }
}

filter {

  if [type] == "netflow" {
  
    if !([netflow][direction] == 0 and [netflow][output_snmp] == 19) {
                drop {}
        }

    geoip {
    	  source => "[netflow][ipv4_dst_addr]"
	      target => "dst_geoip"
	}


	geoip {
          source => "[netflow][ipv4_src_addr]"
          target => "src_geoip"
        }

    if [netflow][protocol] == 6 {
		mutate {
		    add_field => { "IPprotocol" => "tcp" }
		}
	} else if [netflow][protocol] == 14 {
		mutate {
		    add_field => { "IPprotocol" => "udp" }
		}
	} else {
		mutate {
		    add_field => { "IPprotocol" => "na" }
		}
	}

    mutate {
	    add_field => { "Type" => "TRAFFIC" }
	}

    mutate {
        rename => { "[netflow][ipv4_dst_addr]" => "DestinationAddress" }
        rename => { "[netflow][l4_dst_port" => "DestinationPort" }
        rename => { "[netflow][in_bytes]" => "BytesReceived" }
        rename => { "[netflow][out_bytes]" => "BytesSent" }
        rename => { "[netflow][ipv4_src_addr]" => "SourceAddress" }
	rename => { "[netflow][l4_src_port" => "SourcePort" }
    }

    mutate {
        convert => [ "BytesReceived", "integer" ]
        convert => [ "BytesSent", "integer" ]
    }

    ruby {
        code => "event.set('Bytes', 0)"
    }
    ruby {
        code => "event.set('Bytes',event.get('BytesReceived') + event.get('Bytes'))"
    }
    ruby {
        code => "event.set('Bytes',event.get('BytesSent') + event.get('Bytes'))"
    }


    mutate {
	    add_tag => ["Netflow"]
	    add_field => { "SourceDestinationTuple" => "%{SourceAddress}:%{DestinationAddress}:%{DestinationPort}" }
	}

  }
}

output {
  elasticsearch {
    hosts => ["CYBERSIFT_IP_HERE:9200"]
    manage_template => false
    index => "%{[@metadata][beat]}-%{+YYYY.MM.dd}"
    document_type => "%{[@metadata][type]}"
  }
}
```

Notes:
- The above configuration file assumes the correct substitution of CYBERSIFT_IP_HERE
- Pay special attention to **the section in the configuration that probably require modification**:

```
if !([netflow][direction] == 0 and [netflow][output_snmp] == 19) {
                drop {}
        }
```
This section is present to reduce the amount of duplicated netflow sessions. Netflow typically exports connections in a bidirectional manner, i.e. for a single TCP connection there are 2 or more exported flows - one for the inbound connection and one for the outbound connection. Some vendors (depending on configuration) even export before and after NAT flows - resulting in 4 flows which are essentially the same. CyberSift typically expects to see flows in the following format:

| Source Address | Destination Address | Destination Port |
| Private IP | Public Server IP | Corresponding Public Server Port |

For best results, you will need to modify the **direction** and **output_snmp** values depending on your installation. [A full explanation of what the fields represent can be found here](https://www.cisco.com/en/US/technologies/tk648/tk362/technologies_white_paper09186a00800a3db9.html)


- It also assumes that the geoIP plugin is installed using 

```
bin/logstash-plugin install logstash-filter-geoip
```
For further details please review the logstash documentation here: [https://www.elastic.co/guide/en/logstash/current/plugins-filters-geoip.html](https://www.elastic.co/guide/en/logstash/current/plugins-filters-geoip.html)

- We assume that the Logstash version used is **5.6.4**, available to download here:
[https://www.elastic.co/downloads/past-releases/logstash-5-6-4](https://www.elastic.co/downloads/past-releases/logstash-5-6-4)
