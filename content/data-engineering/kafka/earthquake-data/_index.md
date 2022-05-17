---
title: "Earthquake Data"
weight: 100
date: 2022-05-17
---

To gain a better understanding of using Kafka I need to build some projects! Really I need data. So I should go find some data that I can put into Kafka. I have worked with geo-spatial data in the past, and I was impressed by the [US Geological Services earthquake data](https://earthquake.usgs.gov/earthquakes/feed/), so I think that's as good a place to start as any.

## Data Source

- [USGS Eartquake Hazards Program: Real-time Notifications: Spreadsheet Format](https://earthquake.usgs.gov/earthquakes/feed/v1.0/csv.php)

- [Request for ALL EARTHQUAKES over the past hour](https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_hour.csv) (**this link will trigger a download of the past 60 minutes of geo events in CSV format**)

## Plan

1. Make a `curl` request to USGS every 5 minutes via `cron`
1. `python` to:
    1. hash each row of the requested data
    1. compare the has to a store of previously hashed records
    1. if hash does not exist in the store **it has not yet been recorded** so save the hash, and write the row to a new CSV file of `exactly-one-geo-events`
1. start kafka server
1. create kafka topic `geo-events`
1. use `Scala` and `KafkaProducer` to store data in topic
1. use `Scala` and `KafkaConsumer` to read data out of topic, and filter it on earthquakes of a greater than `3.9` magnitude

That would achieve a quasi-stream of USGS reported earthquake data.