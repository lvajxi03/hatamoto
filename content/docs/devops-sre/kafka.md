+++
date = '2025-12-17T22:13:23+02:00'
title = 'Kafka'
menus = 'main'
type = 'page'
+++

Here is the simplest possible way to install and run Apache Kafka locally on Ubuntu, plus a tiny Python producer/consumer example.
This is the cleanest starting point — no Kubernetes, no containers, just Kafka running directly on your machine.

## Install Java (required for Kafka)

Kafka requires a JVM.

```
sudo apt update
sudo apt install -y openjdk-17-jdk
java -version
```

Any Java 11+ is fine.

## Download Kafka (no need to "install" system-wide)

Go to your home directory:

```
cd ~
wget https://downloads.apache.org/kafka/3.7.0/kafka_2.13-3.7.0.tgz
tar -xvf kafka_2.13-3.7.0.tgz
cd kafka_2.13-3.7.0
```

(If there's a newer version, use that—this one works great.)

## Start Kafka (single-node setup)

Kafka needs Zookeeper (for old versions) or can run KRaft mode (newer versions).
The simplest and most compatible option is the classic setup (Zookeeper + Kafka).

In terminal #1 — start Zookeeper:
```
bin/zookeeper-server-start.sh config/zookeeper.properties
```

In terminal #2 — start Kafka broker:
```
bin/kafka-server-start.sh config/server.properties
```

That's it — you have a running Kafka instance.

## Create a test topic

Open a third terminal inside the Kafka directory:

```
bin/kafka-topics.sh --create \
  --topic test-topic \
  --bootstrap-server localhost:9092 \
  --partitions 1 \
  --replication-factor 1
```

Verify:
```
bin/kafka-topics.sh --list --bootstrap-server localhost:9092
```

You should see:
```
test-topic
```

## Python packages to install

For Python, the simplest and most user-friendly Kafka client is kafka-python.

Install it:
```
pip install kafka-python
```

Alternative (more production-grade): `confluent-kafka`, but requires `librdkafka`.
For your case, `kafka-python` is perfectly fine.

## Minimal Python Producer (producer.py)

```python
from kafka import KafkaProducer
import json
import time

producer = KafkaProducer(
    bootstrap_servers="localhost:9092",
    value_serializer=lambda v: json.dumps(v).encode("utf-8")
)

for i in range(10):
    message = {"number": i}
    producer.send("test-topic", message)
    print(f"Sent: {message}")
    time.sleep(1)

producer.flush()
```

Run:
```
python3 producer.py
```

## Minimal Python Consumer (consumer.py)

```python
from kafka import KafkaConsumer
import json

consumer = KafkaConsumer(
    "test-topic",
    bootstrap_servers="localhost:9092",
    value_deserializer=lambda x: json.loads(x.decode("utf-8")),
    auto_offset_reset="earliest",
    enable_auto_commit=True,
    group_id="my-group"
)

print("Listening...")

for message in consumer:
    print("Received:", message.value)
```

Run:
```
python3 consumer.py
```

You should see the numbers printed as they arrive.

## Optional: Run Kafka in the background

To keep Kafka running after you close terminals, use screen, tmux, or run:
```
nohup bin/zookeeper-server-start.sh config/zookeeper.properties > zk.log 2>&1 &
nohup bin/kafka-server-start.sh config/server.properties > kafka.log 2>&1 &
```

## Completely stop Kafka

Use CTRL+C in each terminal, or:
```
bin/kafka-server-stop.sh
bin/zookeeper-server-stop.sh
```

## Summary -- What To Install

| Component   | Command                                  |
| ----------- | ---------------------------------------- |
| Java        | `sudo apt install openjdk-17-jdk`        |
| Kafka       | Download tarball from Apache and extract |
| Python libs | `pip install kafka-python`               |
