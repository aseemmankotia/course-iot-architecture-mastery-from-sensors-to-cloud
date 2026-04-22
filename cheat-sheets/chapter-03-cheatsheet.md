## Chapter 3: Where Intelligence Lives — Quick Reference

### Core Concepts
| Concept | One-line explanation |
|---------|---------------------|
| Edge Computing | Processing data directly at or near sensors/devices for minimal latency |
| Fog Computing | Intermediate layer between edge and cloud; aggregates data from multiple edges |
| Cloud Computing | Centralized processing with unlimited scale but higher latency |
| Edge Gateway | Local hub that filters, preprocesses, and routes data from multiple devices |
| Offline Resilience | Patterns enabling continued operation when cloud connectivity is lost |

### Key Syntax / Commands
```
# Typical Edge-Fog-Cloud Data Flow
Sensor → Edge Device → Edge Gateway → Fog Node → Cloud
        (<1ms)        (1-10ms)      (10-100ms)  (100ms+)

# Data Reduction at Edge (pseudocode)
if (new_reading differs from last_reading by > threshold):
    send_to_cloud(new_reading)
else:
    store_locally(new_reading)  # Batch upload later
```

### Common Patterns
**Pattern 1: Store-and-Forward**
Buffer data locally during connectivity loss; sync when connection restores

**Pattern 2: Tiered Processing**
Simple decisions at edge, aggregations at fog, ML training in cloud

**Pattern 3: Local-First with Cloud Sync**
Process everything locally; upload summaries/anomalies to cloud asynchronously

### Things to Remember
✅ Edge processing can reduce bandwidth costs by up to 80%
✅ Hybrid edge-cloud architecture is the production standard
✅ Sub-millisecond responses require edge-level intelligence
❌ Don't send raw sensor data directly to cloud—filter at the edge first

### Quick Quiz
1. When is fog computing preferred over pure edge? → When aggregating data from multiple edge devices before cloud transmission
2. What's the primary benefit of edge computing for latency? → Enables sub-millisecond response times by eliminating round-trip to cloud