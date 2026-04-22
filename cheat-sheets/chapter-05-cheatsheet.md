## Chapter 5: Taming the Data Firehose — Quick Reference

### Core Concepts
| Concept | One-line explanation |
|---------|---------------------|
| Event Streaming | Continuous flow of data through systems like Kafka or Event Hubs for real-time processing |
| Partition Strategy | Method of distributing data across partitions to balance load and enable parallel processing |
| Hot Partition | Overloaded partition receiving disproportionate traffic, causing bottlenecks |
| Fan-out Pattern | Single message routed to multiple downstream consumers for different processing needs |
| Storage Tiers | Hot (frequent access), Warm (occasional), Cold (archival) based on data age and usage |
| Schema Evolution | Managing changes to data structure over time without breaking consumers |

### Key Syntax / Commands
```
# Partition key selection (pseudo-code)
partition_key = f"{device_region}_{device_type}"  # Good: distributed
partition_key = timestamp  # Bad: creates hot partitions

# Kafka partition count rule of thumb
partitions = max(expected_throughput_MB/s, consumer_count * 2)
```

### Common Patterns
**Pattern 1: Composite Partition Keys**
Combine device_id + time_bucket to spread load while maintaining ordering per device

**Pattern 2: Tiered Storage Pipeline**
Hot (7 days, SSD) → Warm (30 days, HDD) → Cold (years, blob/glacier) with automated lifecycle policies

**Pattern 3: Schema Registry**
Central registry validates producers/consumers against compatible schema versions

### Things to Remember
✅ Choose partition keys that distribute evenly AND align with query patterns
✅ Design for schema changes from day one—use Avro/Protobuf with registry
✅ Set retention policies before launch; storage costs compound silently
❌ Never use high-cardinality timestamps alone as partition keys

### Quick Quiz
1. Why avoid device_id as sole partition key? → Few high-volume devices create hot partitions
2. When to use cold storage? → Data accessed <1x/month, compliance/archival needs