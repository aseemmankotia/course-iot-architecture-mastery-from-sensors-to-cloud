## Chapter 9: Seeing Everything Clearly — Quick Reference

### Core Concepts
| Concept | One-line explanation |
|---------|---------------------|
| Fleet-level metrics vs per-device logging | Aggregates (avg latency, error rates) for health monitoring; detailed logs stored per-device for drill-down |
| Distributed tracing | Correlation IDs that follow a request from sensor → gateway → cloud → response |
| The five meanings of 'offline' | Network down, device sleeping, auth expired, server unreachable, or intentionally disconnected |
| Anomaly detection | Statistical/ML models flagging unusual patterns in operational metrics (drift, spikes, silence) |
| Cost-effective log retention | Tiered storage: hot (7d), warm (30d), cold (1yr); sample verbose logs, keep all errors |

### Key Syntax / Commands
```
# Correlation ID propagation
X-Correlation-ID: {device_id}-{timestamp}-{uuid}

# CloudWatch Insights query pattern
fields @timestamp, device_id, latency
| filter error_code != 0
| stats avg(latency), count() by device_id
| sort count desc | limit 20
```

### Common Patterns
**Pattern 1: Aggregate → Filter → Drill-down**
Start with fleet dashboards, filter to anomalous cohorts, trace specific device logs

**Pattern 2: Structured Logging**
JSON logs with consistent fields: `{device_id, timestamp, event_type, severity, correlation_id}`

### Things to Remember
✅ Instrument correlation IDs at device firmware level—retrofitting is painful
✅ Pre-build queries for common incidents BEFORE you need them
✅ Define "offline" explicitly in your system—each type needs different handling
❌ Don't stream all device logs to cloud in real-time—costs explode at scale

### Quick Quiz
1. Why fleet metrics before device logs? → Aggregates reveal *which* devices to investigate; logs explain *why*
2. Name 3 of the 5 "offline" meanings → Network failure, power/sleep mode, auth/cert expired, server-side issue, intentional disconnect