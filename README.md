# FB.Debezium

This repository contains a sample of Debezium for Event Sourcing.

## Step 1

Up all containers.

```sh
docker-compose up
```

## Step 2

Create a connector in debezium for postgres and kafka.

* Method: POST
* Address: 127.0.0.1:8083/connectors

* Note: If 127.0.0.1 doesn't work, try your static ip. Also change "table.whitelist" property for your table
 

```json
{
  "name": "debezium-test-connector",
  "config": {
    "connector.class": "io.debezium.connector.postgresql.PostgresConnector",
    "tasks.max": "1",
    "group.id": "test",
    "database.hostname": "127.0.0.1",
    "database.port": "5432",
    "database.user": "user",
    "database.password": "password",
    "database.dbname": "debezium",
    "database.server.name": "debezium",
    "database.whitelist": "public",
    "heartbeat.interval.ms": "1000",
    "table.whitelist": "public.users",
    "database.history.kafka.bootstrap.servers": "127.0.0.1:9092",
    "key.converter": "org.apache.kafka.connect.json.JsonConverter",
    "key.converter.schemas.enable": "false",
    "value.converter": "org.apache.kafka.connect.json.JsonConverter",
    "value.converter.schemas.enable": "false",
    "plugin.name": "pgoutput"
  }
}
```

## Step 3

List all topics.

```sh
bin/kafka-topics.sh --list --bootstrap-server localhost:9092
```


## Step 4

Consume a topic.

```sh
bin/kafka-console-consumer.sh --bootstrap-server 127.0.0.1:9092 --from-beginning --topic debezium.public.users | jq
```