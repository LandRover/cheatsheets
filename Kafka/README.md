Kafka Cheatsheet
====


Start / Stop local cluster via Docker
----
```
docker-compose -f docker-compose.yml up -d
docker-compose down
```
```
Creating network "kafka_cluster-lan" with driver "bridge"
Creating kafka_zookeeper-2_1 ... done
Creating kafka_zookeeper-3_1 ... done
Creating kafka_zookeeper-1_1 ... done
Creating kafka_kafka-3_1     ... done
Creating kafka_kafka-2_1     ... done
Creating kafka_kafka-1_1     ... done
```

Docker Cleanup
----
```
docker images
```
```
REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
confluentinc/cp-kafka       latest              64f8db8ddbe5        2 weeks ago         714MB
confluentinc/cp-zookeeper   latest              0667a7f01cf0        2 weeks ago         714MB
```


Docker Cleanup
----
```
docker rmi $(docker images -a -q)
```
```
Untagged: confluentinc/cp-kafka:latest
Untagged: confluentinc/cp-kafka@sha256:b834319bd56bae6fbe2f74a7feb9c2e16312cbdf00fb9cac49f54185211afb35
Deleted: sha256:64f8db8ddbe5377d5844ea486ed9e2483c14021fc236dfc422a7814d6448ab19
Deleted: sha256:74e3a445bbc61f6fc32c0c802af1d66a5079ffe790caa000eeb0774d061d825d
Deleted: sha256:e238813c587d82777487d98b924e5746c37d3ae64ba006b2017d30ef21efd3f5
Untagged: confluentinc/cp-zookeeper:latest
Untagged: confluentinc/cp-zookeeper@sha256:78bb913e38ce6cd335658b6cb424461b04f8a9be95fa90359f900c9d537f7811
Deleted: sha256:0667a7f01cf0389cad84b834f8b4cd845504ffdd766556177d3d8c35eb381adc
Deleted: sha256:6fc6f96dc7c7054e1ec084d46cff55e2cd6bdf44bf85bc03f6952a16465be9e0
Deleted: sha256:f9507e7f5af0d793b20a985b00863267470f598b13df23cb494b1f14452b1dcd
Deleted: sha256:347f72e6b5ce556f08a893f2aa64a29f5294d9db03d46126bd3c6fbadc124bf8
Deleted: sha256:3bbc46df7f5756a4f2359d8e69eccd92b66135c0b189e40ac30ff5aee37f83bf
Deleted: sha256:311e980889dd72bed4aa877558d11e0f593075ae30baba7506689ccc578aca7b
Deleted: sha256:d299d138963e9d2bcb7017c7dee80a037a80248d1d41a71a1e296e1f6d988a1c
Deleted: sha256:13a55052f0e4ca7b4220c4d079515873e36f980fdc23a21421820492cd98b376
Deleted: sha256:761900b2eee363eb4f481982a20e30974d1864a8bd320d9336cfa2a9084d6974
Deleted: sha256:87bdc28bfb8df5f8bd2b5a5ca24daf4f0c2a7a29d24389ae75419f248760164e
Deleted: sha256:26eb5f90e5d91d7d8f65d0a2e18a23eef1afac0016be70ddf8ecc644a93d85f7
Deleted: sha256:d6ec160dc60fbfb962da0b216dfd27f00890bc8ace02994de6441a2517b04db5
```

Docker bash
----
```
docker run -it --net=kafka_cluster-lan --rm confluentinc/cp-kafka:latest bash
```
```
[appuser@b1f27481ae89 ~]$
```


Check Version
----
```
docker run --net=kafka_cluster-lan --rm confluentinc/cp-kafka:latest kafka-topics --version
```
```
6.0.1-ccs (Commit:9c1fbb3db1e0d69d)
```


Topics List
----
```
docker run --net=kafka_cluster-lan --rm confluentinc/cp-kafka:latest kafka-topics --list --zookeeper zookeeper-1:15555
```


Create Topic
----
```
docker run --net=kafka_cluster-lan --rm confluentinc/cp-kafka:latest kafka-topics --create --topic DEMO_TOPIC --partitions 11 --replication-factor 3 --if-not-exists --zookeeper zookeeper-1:15555
```
```
Created topic DEMO_TOPIC.
```

Create Topic with Compact
----
# https://medium.com/swlh/introduction-to-topic-log-compaction-in-apache-kafka-3e4d4afd2262
```
docker run --net=kafka_cluster-lan --rm confluentinc/cp-kafka:latest kafka-topics --create --topic DEMO_TOPIC_COMPACT --partitions 3 --replication-factor 2 --if-not-exists --zookeeper zookeeper-1:15555 --config "cleanup.policy=compact" --config "delete.retention.ms=1680000" --config "segment.ms=100" --config "min.cleanable.dirty.ratio=0.01"
```
```
Created topic DEMO_TOPIC_COMPACT.
```


Topic Describe
----
```
docker run --net=kafka_cluster-lan --rm confluentinc/cp-kafka:latest kafka-topics --bootstrap-server kafka-1:16666 --topic DEMO_TOPIC --describe
```
```
Topic: DEMO_TOPIC	PartitionCount: 11	ReplicationFactor: 3	Configs:
	Topic: DEMO_TOPIC	Partition: 0	Leader: 2	Replicas: 2,1,3	Isr: 2,1,3
	Topic: DEMO_TOPIC	Partition: 1	Leader: 3	Replicas: 3,2,1	Isr: 3,2,1
	Topic: DEMO_TOPIC	Partition: 2	Leader: 1	Replicas: 1,3,2	Isr: 1,3,2
	Topic: DEMO_TOPIC	Partition: 3	Leader: 2	Replicas: 2,3,1	Isr: 2,3,1
	Topic: DEMO_TOPIC	Partition: 4	Leader: 3	Replicas: 3,1,2	Isr: 3,1,2
	Topic: DEMO_TOPIC	Partition: 5	Leader: 1	Replicas: 1,2,3	Isr: 1,2,3
	Topic: DEMO_TOPIC	Partition: 6	Leader: 2	Replicas: 2,1,3	Isr: 2,1,3
	Topic: DEMO_TOPIC	Partition: 7	Leader: 3	Replicas: 3,2,1	Isr: 3,2,1
	Topic: DEMO_TOPIC	Partition: 8	Leader: 1	Replicas: 1,3,2	Isr: 1,3,2
	Topic: DEMO_TOPIC	Partition: 9	Leader: 2	Replicas: 2,3,1	Isr: 2,3,1
	Topic: DEMO_TOPIC	Partition: 10	Leader: 3	Replicas: 3,1,2	Isr: 3,1,2
```

Topic Delete
----
```
docker run --net=kafka_cluster-lan --rm confluentinc/cp-kafka:latest kafka-topics --zookeeper zookeeper-1:15555 --delete --topic DEMO_TOPIC_COMPACT
```
```
Topic DEMO_TOPIC_COMPACT is marked for deletion.
Note: This will have no impact if delete.topic.enable is not set to true.
```


Topic Produce messages - 42 messages
----
```
docker run --net=kafka_cluster-lan --rm confluentinc/cp-kafka:latest bash -c "seq -f 'userid:%02g' 42 | kafka-console-producer --broker-list kafka-1:16666 --topic DEMO_TOPIC --property parse.key=true --property key.separator=":" && echo 'Produced 42 message.'"
```
```
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>Produced 42 message.
```


Topic Consume Simple Messages from the --from-beginning
----
```
docker run --net=kafka_cluster-lan --rm confluentinc/cp-kafka:latest kafka-console-consumer --bootstrap-server kafka-1:16666 --topic DEMO_TOPIC --from-beginning --max-messages 42 --property parse.key=true --property key.separator=":"
```
```
01
02
...
41
42
Processed a total of 42 messages
```

Topic Consume with a ConsumerGroup
----
```
docker run --net=kafka_cluster-lan --rm confluentinc/cp-kafka:latest kafka-console-consumer --bootstrap-server kafka-1:16666 --topic DEMO_TOPIC --group DEMO_TOPIC_GROUP --from-beginning --max-messages 42 --property parse.key=true --property key.separator=":"
```
```
01
02
...
41
42
Processed a total of 42 messages
```

Groups List
----
```
docker run --net=kafka_cluster-lan --rm confluentinc/cp-kafka:latest kafka-consumer-groups --list --bootstrap-server kafka-1:16666
```
```
DEMO_TOPIC_GROUP
```

Groups Describe All
----
```
docker run --net=kafka_cluster-lan --rm confluentinc/cp-kafka:latest kafka-consumer-groups --bootstrap-server kafka-1:16666 --describe --all-groups
```
```
Consumer group 'DEMO_TOPIC_GROUP' has no active members.

GROUP            TOPIC           PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID     HOST            CLIENT-ID
DEMO_TOPIC_GROUP DEMO_TOPIC      9          0               0               0               -               -               -
DEMO_TOPIC_GROUP DEMO_TOPIC      10         0               0               0               -               -               -
DEMO_TOPIC_GROUP DEMO_TOPIC      5          42              42              0               -               -               -
DEMO_TOPIC_GROUP DEMO_TOPIC      6          0               0               0               -               -               -
DEMO_TOPIC_GROUP DEMO_TOPIC      7          0               0               0               -               -               -
DEMO_TOPIC_GROUP DEMO_TOPIC      8          0               0               0               -               -               -
DEMO_TOPIC_GROUP DEMO_TOPIC      1          0               0               0               -               -               -
DEMO_TOPIC_GROUP DEMO_TOPIC      2          0               0               0               -               -               -
DEMO_TOPIC_GROUP DEMO_TOPIC      3          0               0               0               -               -               -
DEMO_TOPIC_GROUP DEMO_TOPIC      4          0               0               0               -               -               -
DEMO_TOPIC_GROUP DEMO_TOPIC      0          0               0               0               -               -               -

Consumer group 'console-consumer-46103' has no active members.
```

Groups Describe - Single
----
```
docker run --net=kafka_cluster-lan --rm confluentinc/cp-kafka:latest kafka-consumer-groups --bootstrap-server kafka-1:16666 --describe --group DEMO_TOPIC_GROUP
```
```
Consumer group 'DEMO_TOPIC_GROUP' has no active members.

GROUP            TOPIC           PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID     HOST            CLIENT-ID
DEMO_TOPIC_GROUP DEMO_TOPIC      9          0               0               0               -               -               -
DEMO_TOPIC_GROUP DEMO_TOPIC      10         0               0               0               -               -               -
DEMO_TOPIC_GROUP DEMO_TOPIC      5          42              42              0               -               -               -
DEMO_TOPIC_GROUP DEMO_TOPIC      6          0               0               0               -               -               -
DEMO_TOPIC_GROUP DEMO_TOPIC      7          0               0               0               -               -               -
DEMO_TOPIC_GROUP DEMO_TOPIC      8          0               0               0               -               -               -
DEMO_TOPIC_GROUP DEMO_TOPIC      1          0               0               0               -               -               -
DEMO_TOPIC_GROUP DEMO_TOPIC      2          0               0               0               -               -               -
DEMO_TOPIC_GROUP DEMO_TOPIC      3          0               0               0               -               -               -
DEMO_TOPIC_GROUP DEMO_TOPIC      4          0               0               0               -               -               -
DEMO_TOPIC_GROUP DEMO_TOPIC      0          0               0               0               -               -               -
```

