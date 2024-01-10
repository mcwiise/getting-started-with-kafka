# Getting started with Kafka Workshop

### Running a single instance

```shell
docker compose -f single-instance.yml up -d --build
# check contaner logs
docker container logs --follow zookeeper
docker container logs --follow kafka
# open 2 terminals to produce and consume a dummy message
docker container exec -it kafka1 /bin/bash
# Start a producer and send messages
kafka-console-producer --bootstrap-server localhost:9092 --topic topic1
# Start a consumer and check messages are read in real time
kafka-console-consumer --bootstrap-server localhost:9092 --topic topic1 --from-beginning
```

### Running a kafka cluster

```shell
docker compose -f multi-broker.yml up -d --build
# check contaner logs
docker container logs --follow zookeeper
docker container logs --follow kafka1
docker container logs --follow kafka2
docker container logs --follow kafka3
# open 2 terminals to produce and consume a dummy message
docker container exec -it kafka1 /bin/bash
# Start a producer and send messages
kafka-console-producer --bootstrap-server localhost:9092 --topic topic1
# Start a consumer and check messages are read in real time
kafka-console-consumer --bootstrap-server localhost:9092 --topic topic1 --from-beginning
```

### Producing messages accross all partitions

```shell
docker container exec -it kafka1 /bin/bash
kafka-topics --bootstrap-server localhost:9092 --topic topic2 --create --partitions 3
kafka-console-producer --bootstrap-server localhost:9092 --producer-property partitioner.class=org.apache.kafka.clients.producer.RoundRobinPartitioner --topic topic2
```

### Producing messages with a message key

```shell
docker container exec -it kafka1 /bin/bash
kafka-topics --bootstrap-server localhost:9092 --topic topic3 --create --partitions 4
kafka-console-producer --bootstrap-server localhost:9092 --topic topic3 --property parse.key=true --property key.separator=:
```

### Consuming messages from a consumer group (idle consumer)

```shell
docker container exec -it kafka1 /bin/bash

# Create a topic with 2 partitions
kafka-topics --bootstrap-server localhost:9092 --topic topic4 --create --partitions 2
# Start a consumer in a consumer group first-app
kafka-console-consumer --bootstrap-server localhost:9092 --topic topic4 --group first-app
# Produce messages round robin
kafka-console-producer --bootstrap-server localhost:9092 --producer-property partitioner.class=org.apache.kafka.clients.producer.RoundRobinPartitioner --topic topic4
# Start another consumer in the same consumer group first-app
# Check messages being spread
kafka-console-consumer --bootstrap-server localhost:9092 --topic topic4 --group first-app
# Start a new consumer and check it gets idle
kafka-console-consumer --bootstrap-server localhost:9092 --topic topic4 --group first-app
```
