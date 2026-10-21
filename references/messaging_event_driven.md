# Messaging, Streaming & Event-Driven Architecture Guide

Use this guide when evaluating message brokers, task queues, pub/sub engines, and event streams.

## Paradigm Classification

1. **Task Queues & Worker Job Pipelines (BullMQ, Celery, Sidekiq, SQS, RabbitMQ Work Queues)**
   * **Intent:** Competing consumer pattern where each job/task is processed exactly once by any available worker.
   * **Key Traits:** Granular task retries, exponential backoff, dead-letter queues (DLQ), priority queues, delayed execution.
   * **When to pick:** Background processing, asynchronous web request offloading, email sending, video rendering.

2. **Smart Broker / Dumb Consumer (RabbitMQ, ActiveMQ, Amazon SQS/SNS)**
   * **Intent:** Routing and discrete message delivery tracking on the broker side.
   * **Key Traits:** Flexible exchange routing (direct, topic, fanout), acknowledgment-based queue cleanup, low message persistence duration after consumption.
   * **When to pick:** Microservice RPC-style decoupled notifications, complex routing rules, discrete events with individual acknowledgments.

3. **Log-Based Distributed Event Streaming (Apache Kafka, Redpanda, AWS Kinesis)**
   * **Intent:** High-throughput, persistent, replayable append-only event log with partitions.
   * **Key Traits:** Dumb broker / smart consumer (consumer tracks its own offset), consumer groups, ordering guaranteed only within a single partition, retention-based deletion.
   * **When to pick:** Event sourcing, audit logging, CQRS event publication, stream processing (Flink), multiple independent consumers needing historical replay.

4. **Lightweight Ultra-Low-Latency Pub/Sub (Redis Pub/Sub, NATS Core / JetStream)**
   * **Intent:** High-performance ephemeral messaging or lightweight persistent streaming (JetStream).
   * **Key Traits:** Extremely low latency (sub-millisecond), simple operational footprint, minimal wire protocol overhead.
   * **When to pick:** Live notifications, WebSocket fanout, distributed cache invalidation, edge-to-cloud telemetry.

## Common Architecture Traps

* **Kafka for Simple Queues:** Attempting to use Kafka as an individual job queue with arbitrary task delays and individual message acknowledgments (partitions block when one item fails).
* **Redis Pub/Sub for Critical Messages:** Standard Redis Pub/Sub has zero persistence—if a consumer disconnects, missed messages are lost forever (prefer Redis Streams or NATS JetStream).
* **Unordered Partition Key Skew in Kafka:** Partitioning by customer or tenant ID where one enterprise tenant generates 80% of events, causing a single consumer thread/partition lag bottleneck.
