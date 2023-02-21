# GoFlow2


This is a fork from https://github.com/netsampler/goflow2 
The upstream goflow2 doesn't support sending the flows to BigQuery database. So the BigQuery transport is added to this repo. 

This application is a NetFlow/IPFIX/sFlow collector in Go.

It gathers network information (IP, interfaces, routers) from different flow protocols,
serializes it in a common format.


![GoFlow2 System diagram](/graphics/diagram.png)

## Get started

To read about agents that samples network traffic, check this [page](/docs/agents.md).

To set up the collector, download the latest release corresponding to your OS
and run the following command (the binaries have a suffix with the version):

```bash
$ ./goflow2
```

By default, this command will launch an sFlow collector on port `:6343` and
a NetFlowV9/IPFIX collector on port `:2055`.

By default, the samples received will be printed in JSON format on the stdout.

```json
{
  "Type": "SFLOW_5",
  "TimeFlowEnd": 1621820000,
  "TimeFlowStart": 1621820000,
  "TimeReceived": 1621820000,
  "Bytes": 70,
  "Packets": 1,
  "SamplingRate": 100,
  "SamplerAddress": "192.168.1.254",
  "DstAddr": "10.0.0.1",
  "DstMac": "ff:ff:ff:ff:ff:ff",
  "SrcAddr": "192.168.1.1",
  "SrcMac": "ff:ff:ff:ff:ff:ff",
  "InIf": 1,
  "OutIf": 2,
  "Etype": 2048,
  "EtypeName": "IPv4",
  "Proto": 6,
  "ProtoName": "TCP",
  "SrcPort": 443,
  "DstPort": 46344,
  "FragmentId": 54044,
  "FragmentOffset": 16384,
  ...
  "IPTTL": 64,
  "IPTos": 0,
  "TCPFlags": 16,
}
```

If you are using a log integration (e.g: Loki with Promtail, Splunk, Fluentd, Google Cloud Logs, etc.),
just send the output into a file.
```bash
$ ./goflow2 -transport.file /var/logs/goflow2.log
```

To enable Kafka and send protobuf, use the following arguments:
```bash
$ ./goflow2 -transport=kafka -transport.kafka.brokers=localhost:9092 -transport.kafka.topic=flows -format=pb
```

By default, the distribution will be randomized.
To partition the feed (any field of the protobuf is available), the following options can be used:
```
-transport.kafka.hashing=true \
-format.hash=SamplerAddress,DstAS

```

To transport the flows to BigQuery use the below options:
```
  -transport=bigquery
  -transport string
    	Choose the transport (available: bigquery, file, kafka) (default "file")
  -transport.bigquery.dataset string
    	BigQuery dataset ID
  -transport.bigquery.project string
    	BigQuery project ID
  -transport.bigquery.table string
    	BigQuery table ID 
```


## License

Licensed under the BSD-3 License.
