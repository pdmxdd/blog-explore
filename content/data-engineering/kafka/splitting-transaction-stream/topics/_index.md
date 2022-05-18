---
title: "Kafka Topics"
weight: 105
date: 2022-05-17
---

Next up starting the zookeeper service and the kafka server.

Then creating my three topics.

## Zookeeper

```bash
bin/zookeeper-server-start.sh config/zookeeper.properties
```

![](pictures/zookeeper.png)

## Kafka

```bash
bin/kafka-server-start.sh config/server.properties
```

![](pictures/kafka.png)

## Transactions Topic

```bash
bin/kafka-topics.sh --create --topic transactions --bootstrap-server localhost:9092
```

![](pictures/kafka-transactions.png)

## Withdrawals Topics

```bash
bin/kafka-topics.sh --create --topic withdrawals --bootstrap-server localhost:9092
```

![](pictures/kafka-withdrawals.png)

## Deposits Topics

```bash
bin/kafka-topics.sh --create --topic deposits --bootstrap-server localhost:9092
```

![](pictures/kafka-deposits.png)

## Topic Validations

After creating all of the topics I ran:

```bash
bin/kafka-topics.sh --list --bootstrap-server localhost:9092
```

And got the following:

![](pictures/all-topics.png)

They're all here! As is the geo-events topic I created for my last project. Sadly buddy you won't be seeing any events right now!