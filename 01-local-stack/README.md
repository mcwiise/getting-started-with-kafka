# Kafka as Docker Container

### Running a single instance

```shell
cd single-instance
docker-compose up -d --build
# check contaner logs
docker container logs --follow zookeeper3
docker container logs --follow kafka3
# open 2 terminals to produce and consume a dummy message
docker container exec -u root -it kafka3 /bin/bash
kafka-console-producer.sh --bootstrap-server localhost:9092 --topic first_topic
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic first_topic --from-beginning
```

### Running a multi broker cluster

```shell
cd multi-broker
docker-compose up -d --build
# check contaner logs
docker container logs --follow zookeeper3
docker container logs --follow kafka31
docker container logs --follow kafka32
docker container logs --follow kafka33
# open 2 terminals to produce and consume a dummy message
docker container exec -u root -it kafka31 /bin/bash
kafka-console-producer.sh --bootstrap-server localhost:7071 --topic first_topic
kafka-console-consumer.sh --bootstrap-server localhost:7071 --topic first_topic --from-beginning
```
