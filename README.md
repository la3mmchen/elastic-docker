# Elastic Stack on Docker using Kafka
This is a docker-compose file to deploy an Elastic Stack.

```
docker-compose up
```

This setup uses Redis as a local message broker on top of Logstash. Thus, the pipeline is:
* filebeat watches for logs in "/*.log" and "/var/log/*.log" and ships this messages to a Redis
* Logstash retrieves messages from Redis and puts them into Elasticsearch index "via_redis"
* Kibana is listening to localhost:8888


Included is a monitoring solution (Prometheus and Grafana)
* Grafana: localhost:3000
* Prometheus localhost:9090
