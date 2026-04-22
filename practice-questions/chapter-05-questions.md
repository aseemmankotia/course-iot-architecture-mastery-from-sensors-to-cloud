## Chapter 5: Taming the Data Firehose — Practice Questions

### Multiple Choice

**Q1.** In an event streaming architecture, what is the primary purpose of partitioning data across multiple partitions?

A) To reduce the total storage cost of the system
B) To enable parallel processing and increase throughput
C) To encrypt data in transit between producers and consumers
D) To automatically convert data between different schema formats

<details>
<summary>Answer</summary>

**Correct: B**

Partitioning enables parallel processing by allowing multiple consumers to read from different partitions simultaneously, dramatically increasing throughput. Storage costs (A) are not primarily affected by partitioning decisions. Encryption (C) is handled separately from partitioning. Schema conversion (D) is a separate concern unrelated to partition structure.

</details>

---

**Q2.** A fleet of 10,000 IoT devices sends telemetry using their device ID as the partition key. You notice that 80% of messages are landing in just 3 of your 32 partitions. What is the most likely cause?

A) The consumer group has too few members
B) The partition key distribution is skewed due to non-uniform device ID patterns
C) The storage tier is configured incorrectly
D) Schema evolution has corrupted the routing logic

<details>
<summary>Answer</summary>

**Correct: B**

Hot partitions typically occur when partition keys are not uniformly distributed. If device IDs follow a pattern (e.g., sequential numbering or regional prefixes), the hash function may map many keys to the same partitions. Consumer group size (A) affects processing but not message distribution. Storage configuration (C) and schema evolution (D) do not impact partition assignment.

</details>

---

**Q3.** Which storage tier strategy is most appropriate for IoT telemetry data that needs to be queried frequently for the first 7 days, occasionally for the next 90 days, and rarely accessed thereafter?

A) Store all data in hot storage indefinitely for consistent performance
B) Use hot storage for 7 days, warm storage for 90 days, then cold storage
C) Use cold storage exclusively with caching for recent queries
D) Delete data after 7 days to minimize storage costs

<details>
<summary>Answer</summary>

**Correct: B**

A tiered approach matches storage costs and performance characteristics to actual access patterns. Hot storage provides fast access for frequently queried recent data, warm storage balances cost and performance for occasional access, and cold storage minimizes costs for archival data. Option A is unnecessarily expensive, option C would have poor performance for frequent queries, and option D loses valuable historical data.

</details>

---

**Q4.** When implementing a fan-out pattern for IoT telemetry, which approach ensures that multiple downstream consumers can independently process the same messages?

A) Use a single consumer that duplicates messages to each downstream service
B) Configure each downstream service to share the same consumer group
C) Create separate consumer groups for each downstream processing pipeline
D) Store messages in cold storage and have services query on demand

<details>
<summary>Answer</summary>

**Correct: C**

Separate consumer groups allow each downstream pipeline to maintain its own offset and process all messages independently. A single duplicating consumer (A) creates a bottleneck and single point of failure. Sharing a consumer group (B) would distribute messages across consumers rather than duplicate them. Cold storage queries (D) introduce latency and don't support real-time processing.

</details>

---

**Q5.** You need to add a new optional field to your IoT telemetry schema without breaking existing consumers. Which approach supports backward-compatible schema evolution?

A) Delete the old schema and deploy the new one simultaneously with consumer updates
B) Use a schema format like Avro with optional fields and a schema registry
C) Send data as unstructured text to avoid schema constraints
D) Create an entirely new topic for the new schema version

<details>
<summary>Answer</summary>

**Correct: B**

Avro and similar formats with schema registries support backward-compatible evolution by allowing optional fields with defaults. Old consumers ignore new fields, while new consumers can handle both old and new data. Simultaneous deployment (A) risks downtime. Unstructured text (C) loses validation benefits and complicates processing. New topics (D) fragment data and complicate consumer logic.

</details>

---

### True / False

**Q6.** Using a timestamp as the sole partition key for IoT telemetry is a reliable strategy for ensuring even data distribution across partitions. — **True / False**

**False.** Using timestamps as partition keys typically causes hot partitions because all devices sending data at the same time will hash to the same partition. This creates temporal clustering where the "current" partition receives disproportionate traffic. A better approach combines device ID with timestamp or uses a composite key to distribute load evenly.

---

**Q7.** In Apache Kafka and Azure Event Hubs, messages within a single partition are guaranteed to be processed in the order they were produced. — **True / False**

**True.** Both Kafka and Event Hubs guarantee ordering within a partition. Messages sent to the same partition maintain their sequence, which is critical for IoT scenarios where event ordering matters (e.g., tracking device state changes). However, ordering is not guaranteed across different partitions.

---

### Short Answer

**Q8.** Explain the difference between at-least-once and exactly-once delivery semantics in event streaming, and describe a scenario where each would be appropriate for IoT data processing.

<details>
<summary>Answer</summary>

**At-least-once delivery** guarantees messages are delivered but may result in duplicates if acknowledgments fail. This is appropriate for IoT scenarios where duplicate processing is acceptable or easily handled, such as aggregating temperature readings where processing the same reading twice has minimal impact.

**Exactly-once delivery** guarantees each message is processed precisely once, with no duplicates or losses. This is critical for IoT scenarios involving state changes or financial transactions, such as tracking the exact count of items passing through a sensor on a manufacturing line, where duplicates would corrupt inventory counts.

</details>

---

**Q9.** Describe three strategies for preventing hot partitions when ingesting data from a large fleet of IoT devices.

<details>
<summary>Answer</summary>

1. **Use composite partition keys**: Combine device ID with a random suffix or time bucket to spread load from high-volume devices across multiple partitions.

2. **Apply salting or key hashing**: Add a random component to partition keys or use consistent hashing algorithms that distribute keys more uniformly across available partitions.

3. **Implement write sharding**: For devices that produce unusually high volumes, programmatically distribute their messages across multiple partition keys (e.g., deviceId-0, deviceId-1) and aggregate during consumption.

Additional strategies include monitoring partition metrics to identify skew early and rebalancing partition assignments when hot spots emerge.

</details>

---

### Scenario-Based

**Q10.** Your company operates 50,000 smart agricultural sensors that report soil moisture, temperature, and pH levels every 5 minutes. During irrigation season, certain regions experience 10x normal message volume. You currently use a single Event Hub with 8 partitions, partitioned by sensor ID. The system is experiencing significant lag during peak periods, and some messages are being dropped due to throughput limits.

Design an improved ingestion architecture that addresses these issues. Include your recommendations for partition strategy, scaling approach, and how you would handle the seasonal traffic spikes.

<details>
<summary>Answer</summary>

**Recommended Architecture:**

**1. Increase Partition Count:**
Scale from 8 to at least 32 partitions to enable greater parallelism. Calculate based on peak throughput: 50,000 sensors × 10x peak factor ÷ 5 minutes = ~1.6 million messages per minute at peak. Each partition should handle a manageable subset.

**2. Revise Partition Strategy:**
Use a composite key combining region ID and sensor ID (e.g., `region-sensorId`). This prevents hot partitions when specific regions experience peak irrigation while maintaining message ordering per sensor.

**3. Implement Auto-Scaling Consumer Groups:**
Deploy consumers that scale horizontally based on partition lag metrics. Use container orchestration (Kubernetes) to automatically add consumer instances when lag exceeds thresholds.

**4. Add a Buffering Layer:**
Implement edge gateways in each agricultural region that can buffer messages locally during extreme spikes and smooth out transmission rates to the central Event Hub.

**5. Tiered Processing:**
Route messages to different processing paths based on urgency. Critical alerts (sensor failures, extreme readings) go to a fast path, while routine telemetry can tolerate slight delays through a standard path.

**6. Seasonal Capacity Planning:**
Pre-scale resources before irrigation season based on historical patterns. Use throughput units auto-inflate feature in Event Hubs to handle unexpected spikes.

</details>