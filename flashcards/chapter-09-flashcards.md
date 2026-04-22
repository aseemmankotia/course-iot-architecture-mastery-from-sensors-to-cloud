## Chapter 9: Seeing Everything Clearly — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Why are fleet-level metrics preferred over per-device logging at scale? | Aggregates reveal systemic patterns, reduce data volume/cost, and enable faster diagnosis—you can't examine millions of individual device logs during an incident. |
| 2 | What is distributed tracing in the IoT context? | A technique that tracks a single request/event as it flows from device through gateways, cloud services, and backends using correlated trace IDs. |
| 3 | What makes distributed tracing challenging across the device-cloud boundary? | Constrained devices may lack resources for trace propagation, network gaps interrupt trace continuity, and clock synchronization issues affect timing accuracy. |
| 4 | What are the five meanings of 'offline' in IoT systems? | 1) No network connectivity, 2) Connected but not sending data, 3) Sending but cloud not receiving, 4) Device powered off, 5) Device unresponsive to commands. |
| 5 | Why is distinguishing between different 'offline' states important? | Each state requires different diagnostic approaches and remediation—a connectivity issue vs. a crashed application need entirely different responses. |
| 6 | What is anomaly detection on operational metrics? | Using statistical methods or ML to automatically identify unusual patterns in device behavior, resource usage, or communication patterns that may indicate problems. |
| 7 | What key metrics should anomaly detection monitor in IoT fleets? | Message frequency, payload sizes, error rates, battery drain rates, memory usage, response latencies, and connection/reconnection patterns. |
| 8 | What does "observability as architecture" mean? | Designing logging, metrics, and tracing into the system from the start rather than bolting it on later—making instrumentation a first-class concern. |
| 9 | What is a cost-effective log retention strategy for IoT? | Tiered storage: hot storage for recent/critical logs, warm storage for weeks-old data, cold/archive for compliance needs; aggressive aggregation and sampling. |
| 10 | When should you drill down from fleet metrics to individual device traces? | When aggregate metrics show anomalies, customer reports issues, or you need to understand the specific sequence of events for a particular failure. |
| 11 | What is the "diagnose with aggregates, drill down with traces" approach? | Start investigations with fleet-wide statistical views to identify scope/patterns, then use detailed traces only for specific devices showing problems. |
| 12 | How does sampling help with IoT observability costs? | Collecting detailed traces/logs from only a percentage of devices or events while maintaining statistical validity for fleet-wide insights. |
| 13 | Why should observability queries be optimized before incidents occur? | During outages, you need answers in seconds—pre-built dashboards and indexed queries eliminate the delay of crafting ad-hoc queries under pressure. |
| 14 | What is cardinality explosion in IoT metrics? | When unique metric combinations (device ID × metric × tags) grow exponentially with fleet size, overwhelming time-series databases and budgets. |
| 15 | How can edge aggregation reduce observability costs? | Devices or gateways compute summaries (min/max/avg/count) locally and send only aggregates, dramatically reducing data transmission and storage needs. |

### Key Terms

| Term | Definition |
|------|-----------|
| Fleet-level metrics | Aggregated measurements across all devices that reveal system-wide health and trends rather than individual device states. |
| Distributed tracing | Tracking technique using correlated IDs to follow requests across multiple services and system boundaries. |
| Trace ID | Unique identifier propagated through all components handling a single request, enabling end-to-end visibility. |
| Anomaly detection | Automated identification of data points or patterns that deviate significantly from expected behavior. |
| Log retention policy | Rules governing how long different types of logs are stored and in what storage tier based on value and compliance needs. |
| Observability | The ability to understand a system's internal state by examining its external outputs (logs, metrics, traces). |
| Cardinality | The number of unique values for a metric dimension; high cardinality dramatically increases storage and query costs. |

### Memory Tricks
- **FIVE offline states = "CROPS"**: Connected-not-sending, Receiving-blocked, Off-powered, Protocol-connected-unresponsive, Signal-lost
- **Observability pillars = "LMT" (Like Mountain Time)**: Logs, Metrics, Traces—the three pillars you need across all time zones of your fleet
- **"Aggregate first, trace second"** = Like a doctor checking vitals before ordering an MRI—start broad, then go deep only where needed