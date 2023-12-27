# Kafka as Docker Container

### Running a single instance

```shell
cd 01-local-stack
docker compose -f single-instance.yml up -d --build
# check contaner logs
docker container logs --follow zookeeper
docker container logs --follow kafka
# open 2 terminals to produce and consume a dummy message
docker container exec -it kafka /bin/bash
kafka-console-producer --bootstrap-server localhost:9092 --topic first_topic
kafka-console-consumer --bootstrap-server localhost:9092 --topic first_topic --from-beginning
```

### Running a multi broker cluster

```shell
cd 01-local-stack
docker compose -f multi-broker.yml up -d --build
# check contaner logs
docker container logs --follow zookeeper
docker container logs --follow kafka1
docker container logs --follow kafka2
docker container logs --follow kafka3
# open 2 terminals to produce and consume a dummy message
docker container exec -it kafka1 /bin/bash
kafka-console-producer --bootstrap-server localhost:9092 --topic first_topic
kafka-console-consumer --bootstrap-server localhost:9092 --topic first_topic --from-beginning
```
