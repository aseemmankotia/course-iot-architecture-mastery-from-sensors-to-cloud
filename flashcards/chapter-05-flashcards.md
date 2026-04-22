## Chapter 5: Taming the Data Firehose — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | What is an event streaming architecture? | A data processing paradigm where events (records of state changes) flow continuously through distributed systems like Event Hubs or Kafka, enabling real-time processing at scale. |
| 2 | What is the primary difference between Azure Event Hubs and Apache Kafka? | Event Hubs is a managed PaaS service with Kafka-compatible API, while Kafka is open-source requiring self-management. Event Hubs offers automatic scaling; Kafka offers more customization. |
| 3 | What is a partition in event streaming? | A logical subdivision of a topic/event hub that enables parallel processing. Each partition is an ordered, immutable sequence of events that can be consumed independently. |
| 4 | What causes a "hot partition" problem? | When a poorly chosen partition key causes uneven distribution, routing disproportionate traffic to one partition, creating a bottleneck while other partitions sit idle. |
| 5 | How do you prevent hot partitions in IoT telemetry? | Use high-cardinality partition keys (device ID + timestamp hash), avoid using only region or device type, and consider round-robin for uniform distribution when ordering isn't critical. |
| 6 | What is a fan-out pattern in message routing? | A pattern where a single incoming message is duplicated and sent to multiple downstream consumers or processing pipelines simultaneously. |
| 7 | What is the difference between hot, warm, and cold storage tiers? | Hot: frequently accessed, lowest latency, highest cost. Warm: occasional access, moderate latency/cost. Cold: rare access (archival), highest latency, lowest cost. |
| 8 | When should IoT telemetry data move from hot to warm storage? | Typically after 7-30 days when real-time access is no longer needed but data must remain queryable for operational dashboards or recent trend analysis. |
| 9 | What is schema evolution? | The ability to change data structure (add/remove fields) over time while maintaining backward and/or forward compatibility with existing consumers and stored data. |
| 10 | Why is Avro commonly preferred over JSON for high-volume IoT streaming? | Avro offers compact binary serialization, schema registry integration, built-in schema evolution support, and significantly smaller payload sizes than verbose JSON. |
| 11 | What is backward compatibility in schema evolution? | New schema can read data written with the old schema. Achieved by providing default values for new fields. |
| 12 | What is a dead letter queue in message routing? | A holding area for messages that cannot be processed successfully, allowing failed events to be analyzed and reprocessed without blocking the main pipeline. |
| 13 | How does consumer group scaling work in Kafka/Event Hubs? | Each partition can only be read by one consumer per group. Maximum parallelism equals partition count—adding more consumers than partitions leaves them idle. |
| 14 | What financial risk does poor partition strategy create? | Hot partitions require expensive vertical scaling, wasted capacity on cold partitions, increased cloud costs, and performance issues that compound over 6+ months of data growth. |
| 15 | What is the recommended approach for partition count planning? | Over-provision partitions initially (can't easily reduce later), estimate peak throughput × 3-5x growth, and align with expected consumer parallelism requirements. |

### Key Terms

| Term | Definition |
|------|-----------|
| Partition Key | The field used to determine which partition receives an event; critical for even distribution and ordering guarantees |
| Consumer Group | A named group of consumers that collectively read from all partitions, enabling parallel processing and load balancing |
| Schema Registry | A centralized service storing and managing schemas, enabling producers and consumers to validate and evolve data formats |
| Throughput Units | Azure Event Hubs capacity metric; each TU provides 1 MB/s ingress and 2 MB/s egress |
| Event Retention | The duration events remain available in the stream before automatic deletion (hours to days depending on tier) |
| Compaction | A Kafka feature that retains only the latest value per key, useful for maintaining current device state |
| Checkpointing | Recording consumer progress through partitions to enable resumption after failures without data loss or duplication |

### Memory Tricks
- **HOT partitions = HOT mess**: Remember that uneven load creates expensive problems—visualize one server on fire while others are idle
- **"ABC" storage tiers**: Active (hot), Backup (warm), Cheap (cold)—data flows down the alphabet as it ages
- **"PEACE" for partition keys**: Pick high-cardinality, Evenly distributed, Avoid low-variety, Consider ordering needs, Estimate growth