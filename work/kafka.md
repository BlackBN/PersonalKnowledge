# kafka命令

```bash
docker exec -it kafka /bin/bash

cd /bin

kafka-topics.sh --create --topic test --zookeeper zookeeper:2181 --replication-factor 1 --partitions 1

kafka-topics.sh --zookeeper zookeeper:2181 --describe --topic test

kafka-console-producer.sh --topic=test --broker-list kafka:9092

kafka-console-consumer.sh --bootstrap-server kafka:9092 --from-beginning --topic test

find / -name \*kafka_\* | head -1 | grep -o '\kafka[^\n]*' #查看版本号

kafka-topics.sh --zookeeper host:2181 --list

```
