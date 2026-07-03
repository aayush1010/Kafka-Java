# Kafka + Spring Boot

## What this is

A hands-on Kafka setup in plain Java: a producer, a consumer, and a real-world data source (Twitter's streaming API) feeding the producer instead of a hardcoded message.

## Design

- **`Producer`** — configures `KafkaProducer` with bootstrap servers + string (de)serializers, sends a `ProducerRecord` to `first_topics`, and flushes/closes cleanly.
- **`Consumer`** — configures `KafkaConsumer` with a consumer group (`my-app`) and `earliest` offset reset, subscribes to `first_topics`, and polls in a loop.
- **`TwitterProducer`** — the interesting piece: opens a filtered Twitter stream via the Hosebird Client (`hbc`), buffers incoming tweets on a `BlockingQueue`, and republishes each one onto the `twitter_tweets` Kafka topic through a `KafkaProducer`, with an async callback for send errors.

## Tech

Java, Apache Kafka client, Twitter Hosebird Client (hbc), Maven

## How to Run

1. Start a local Kafka broker on `127.0.0.1:9092`
2. Run `Consumer` to subscribe to the topic you care about (`first_topics` or `twitter_tweets`)
3. Run `Producer` for a simple hello-world message, or `TwitterProducer` (with Twitter API credentials) to stream live tweets into Kafka
