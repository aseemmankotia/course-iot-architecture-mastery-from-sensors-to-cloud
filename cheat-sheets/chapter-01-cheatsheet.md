## Chapter 1: The Connected World Blueprint — Quick Reference

### Core Concepts
| Concept | One-line explanation |
|---------|---------------------|
| Three-Layer Model | Simplified architecture: Perception → Network → Application |
| Five-Layer Model | Extended architecture adding Processing and Business layers |
| Device-Gateway-Cloud | Reference pattern where devices collect, gateways aggregate, cloud processes |
| Perception Layer | Physical sensors/actuators that interact with the real world |
| Network Layer | Handles data transmission between devices and cloud |
| Application Layer | User-facing services and domain-specific logic |
| CAP Theorem | Distributed systems can only guarantee 2 of 3: Consistency, Availability, Partition tolerance |

### Key Architecture Flow
```
[Sensors/Devices] → [Gateway] → [Cloud Platform]
     ↓                  ↓              ↓
  Collect           Aggregate       Process
  Measure           Translate       Store
  Actuate           Filter          Analyze
```

### Common Patterns
**Pattern 1: Edge Aggregation**
Gateway collects data from multiple devices, filters/compresses, then sends batched data to cloud to reduce bandwidth.

**Pattern 2: Store-and-Forward**
Gateway buffers data locally during network outages, syncs when connectivity restored—handles partition tolerance.

### Things to Remember
✅ Always design with separation of concerns—each layer has distinct responsibilities
✅ Gateways are critical for protocol translation (Zigbee/BLE → MQTT/HTTP)
✅ IoT systems typically favor Availability + Partition tolerance over strong Consistency
❌ Don't send raw sensor data directly to cloud—aggregate at the edge first

### Quick Quiz
1. Which CAP properties do most IoT systems prioritize? → **AP** (Availability & Partition tolerance)
2. What layer handles sensor data collection? → **Perception layer**
3. Primary role of a gateway? → **Aggregate, translate protocols, filter data**