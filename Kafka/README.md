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
docker run --network=host --rm confluentinc/cp-kafka:latest kafka-topics --version
```


Topics List
----
```
docker run --network=host --rm confluentinc/cp-kafka:latest kafka-topics --list --zookeeper localhost:15555
```


Create Topic
----
```
docker run --network=host --rm confluentinc/cp-kafka:latest kafka-topics --create --topic DEMO_TOPIC --partitions 11 --replication-factor 3 --if-not-exists --zookeeper localhost:15555
```


Create Topic with Compact
----
```
# https://medium.com/swlh/introduction-to-topic-log-compaction-in-apache-kafka-3e4d4afd2262
docker run --network=host --rm confluentinc/cp-kafka:latest kafka-topics --create --topic DEMO_TOPIC_COMPACT --partitions 3 --replication-factor 2 --if-not-exists --zookeeper localhost:15555 --config "cleanup.policy=compact" --config "delete.retention.ms=1680000" --config "segment.ms=100" --config "min.cleanable.dirty.ratio=0.01"
```


Topic Describe
----
```
docker run --network=host --rm confluentinc/cp-kafka:latest kafka-topics --bootstrap-server localhost:26666 --topic DEMO_TOPIC --describe
```


Topic Delete
----
```
# https://medium.com/swlh/introduction-to-topic-log-compaction-in-apache-kafka-3e4d4afd2262
docker run --network=host --rm confluentinc/cp-kafka:latest kafka-topics --zookeeper localhost:15555 --delete --topic DEMO_TOPIC_COMPACT
```


Topic Produce messages - 42 messages
----
```
docker run --network=host --rm confluentinc/cp-kafka:latest bash -c "seq -f 'userid:%02g' 42 | kafka-console-producer --broker-list localhost:36666 --topic DEMO_TOPIC --property parse.key=true --property key.separator=":" && echo 'Produced 42 message.'"
```


Topic Consume Simple Messages from the --from-beginning
----
```
docker run --network=host --rm confluentinc/cp-kafka:latest kafka-console-consumer --bootstrap-server localhost:26666 --topic DEMO_TOPIC --new-consumer --from-beginning --max-messages 42 --property parse.key=true --property key.separator=":"
```


Topic Consume with a ConsumerGroup
----
```
docker run --network=host --rm confluentinc/cp-kafka:latest kafka-console-consumer --bootstrap-server localhost:26666 --topic DEMO_TOPIC --group DEMO_TOPIC_GROUP --from-beginning --max-messages 42 --property parse.key=true --property key.separator=":"
```


Groups List
----
```
docker run --network=host --rm confluentinc/cp-kafka:latest kafka-consumer-groups --list --bootstrap-server localhost:26666
```


Groups Describe All
----
```
docker run --network=host --rm confluentinc/cp-kafka:latest kafka-consumer-groups --bootstrap-server localhost:26666 --describe --all-groups
```


Groups Describe - Single
----
```
docker run --network=host --rm confluentinc/cp-kafka:latest kafka-consumer-groups --bootstrap-server localhost:26666 --describe --group DEMO_TOPIC_GROUP
```

