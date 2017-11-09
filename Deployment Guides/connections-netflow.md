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
- It also assumes that the geoIP plugin is installed using 

```
bin/logstash-plugin install logstash-filter-geoip
```
For further details please review the logstash documentation here: [https://www.elastic.co/guide/en/logstash/current/plugins-filters-geoip.html](https://www.elastic.co/guide/en/logstash/current/plugins-filters-geoip.html)

- We assume that the Logstash version used is **5.2.1**, available to download here:
[https://www.elastic.co/downloads/past-releases/logstash-5-2-1](https://www.elastic.co/downloads/past-releases/logstash-5-2-1)
