# Elastic Stack on Docker using Kafka
This is a docker-compose file to deploy an Elastic Stack.

This setup uses Kafka as message broker. The pipeline is:
* filebeat watches for logs in "/*.log" and ships this messages to Apache Kafka topic "applog"
* filebeat watches for logs in "/var/log/*.log" and ships this messages to Kafka topic "systemlog"
* Logstash retrieves messages from topic "applog", decodes the json into Logstash objects and puts them into Elasticsearch index "via_kafka"
* Kibana is listening to localhost:8888
* Creating data: docker cp someJsonLogFile.log elasticdocker_filebeat_1:/

Troubleshooting:
* If you've got problems to start it the second time: it may be helpful to delete the zookeeper container as it is caching the kafka broker id. 
