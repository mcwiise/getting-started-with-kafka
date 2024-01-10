# Getting started with Kafka Workshop

### Running a single instance

```shell
docker compose -f single-instance.yml up -d --build
# check contaner logs
docker container logs --follow zookeeper
docker container logs --follow kafka
# open 2 terminals to produce and consume a dummy message
docker container exec -it kafka1 /bin/bash
kafka-console-producer --bootstrap-server localhost:9092 --topic topic1
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
kafka-console-producer --bootstrap-server localhost:9092 --topic topic1
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

###
