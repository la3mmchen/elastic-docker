# Elastic Stack on Docker using Kafka
This is a docker-compose file to deploy an Elastic Stack. (Elastic Stack Version 6)

Thus, the pipeline is:
* filebeat watches for logs in "/tmp/logs.*log" and ships this messages to Kafka.
* Logstash retrieves messages from Kafka and puts them into Elasticsearch index
* Kibana is listening to localhost:5601

## Startup
```
    docker-compose up
```

## Setup initial password
Once all the container are started exec  into the elastic-1 container and setup the inital passwords:
```
    docker exec -it ws_elastic-1 /bin/bash
    % /usr/share/elasticsearch/bin/x-pack/setup-passwords interactive
```
(Background https://www.elastic.co/guide/en/x-pack/current/setting-up-authentication.html#set-built-in-user-passwords)

## Add Logstash Pipelines:
Create pipelines via Kibana > Management > Logstash > Pipelines with the files under `logstash/eventpipe_*`
See `logstash/logstash.yml` how to they have to be named.

## Restart
Sometimes the setup needs some kicking to restart properly. especially Zookeeper seems to be tricky:
```
    docker rm ws_zookeeper-1 ws_kafka-1 ws_filebeat-1 ws_metricbeat-1
```


## Cheat Sheet

### Kafka 
Kafkacat: read topic stream
```shell
kafkacat -C -t filebeat-log -b localhost:9092
```
Kafkacat: See all topics on broker
```shell
kafkacat -L -b localhost:9092
```

Kafka-Offset Check:
```shell
/usr/local/bin/kt group -brokers localhost:9092 -topic filebeat-log -group logstash-applog
```

### Curator
```
/usr/bin/curator --config /tmp/curator.yml /tmp/delete_old.yml
```

### Filebeat Generate Logs:
Use: https://github.com/bitsofinfo/log-generator
```
~/repos/github/log-generator/log_generator.py --sourceDataFile ~/repos/github/log-generator/defaultDataFile.txt --logFile ./filebeat/logs/demo.log
```

