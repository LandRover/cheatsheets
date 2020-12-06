Kafka Cheatsheet
====


Start / Stop local cluster via Docker
----
```
docker-compose -f docker-compose.yml up -d
docker-compose down
```


Docker Cleanup
----
```
docker rmi $(docker images -a -q)
```


Check Version
----
```
docker run --net=host --rm confluentinc/cp-kafka:latest kafka-topics --version
```


Create Topic
----
```
docker run --net=host --rm confluentinc/cp-kafka:latest kafka-topics --create --topic DEMO_TOPIC --partitions 11 --replication-factor 3 --if-not-exists --zookeeper localhost:15555
```


Create Topic with Compact
----
```
# https://medium.com/swlh/introduction-to-topic-log-compaction-in-apache-kafka-3e4d4afd2262
docker run --net=host --rm confluentinc/cp-kafka:latest kafka-topics --create --topic DEMO_TOPIC_COMPACT --partitions 3 --replication-factor 2 --if-not-exists --zookeeper localhost:15555 --config "cleanup.policy=compact" --config "delete.retention.ms=1680000" --config "segment.ms=100" --config "min.cleanable.dirty.ratio=0.01"
```

Create Describe
----
```
docker run --net=host --rm confluentinc/cp-kafka:latest kafka-topics --bootstrap-server localhost:26666 --topic DEMO_TOPIC --describe
```
