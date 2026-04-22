## Chapter 2: Speaking the Right Language — Quick Reference

### Core Concepts
| Concept | One-line explanation |
|---------|---------------------|
| MQTT | Lightweight publish-subscribe protocol over TCP, ideal for unreliable networks |
| QoS 0 | Fire-and-forget: no delivery guarantee, lowest overhead |
| QoS 1 | At-least-once: acknowledged delivery, possible duplicates |
| QoS 2 | Exactly-once: guaranteed single delivery, highest overhead |
| CoAP | REST-like protocol over UDP for severely constrained devices |
| AMQP | Enterprise-grade messaging with queuing, routing, and transactions |
| TLS | Encryption layer for TCP-based protocols (MQTT, AMQP) |
| DTLS | Encryption layer for UDP-based protocols (CoAP) |

### Key Syntax / Commands
```
MQTT Topic Structure:    sensors/{building}/{floor}/{device_id}/temperature
MQTT Wildcards:          + (single level)    # (multi-level)

CoAP Methods:            GET, PUT, POST, DELETE (like REST)
CoAP Observe:            Subscribe to resource changes without polling

AMQP Components:         Exchange → Binding → Queue → Consumer
```

### Common Patterns
**Pattern 1: MQTT Broker Topology**
Devices publish to topics → Broker routes → Subscribers receive. Decouples senders from receivers.

**Pattern 2: CoAP Request/Response**
Client sends confirmable (CON) or non-confirmable (NON) request → Server responds with ACK + payload.

### Things to Remember
✅ MQTT + QoS 1 covers 90% of IoT use cases (reliable, reasonable overhead)
✅ CoAP shines when RAM < 10KB or battery life is critical (UDP = smaller packets)
✅ AMQP for enterprise integration requiring message queuing and transactions
❌ Don't use QoS 2 by default—the 4-way handshake adds significant latency

### Quick Quiz
1. When choose CoAP over MQTT? → Extremely constrained devices, need REST semantics, or UDP preferred
2. What's the tradeoff of QoS 2? → Guarantees exactly-once delivery but requires 4 packets per message