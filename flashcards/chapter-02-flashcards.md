## Chapter 2: Speaking the Right Language — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | What communication pattern does MQTT use? | Publish-subscribe model where clients publish messages to topics and subscribe to receive messages from topics via a central broker |
| 2 | What transport protocol does MQTT run over? | TCP (Transmission Control Protocol), providing reliable ordered delivery |
| 3 | What are the three MQTT QoS levels? | QoS 0: At most once (fire and forget), QoS 1: At least once (acknowledged delivery), QoS 2: Exactly once (four-step handshake) |
| 4 | When would you choose MQTT QoS 0? | When occasional message loss is acceptable, such as frequent sensor readings where the next update will arrive soon |
| 5 | What transport protocol does CoAP use? | UDP (User Datagram Protocol), making it lightweight but requiring application-level reliability mechanisms |
| 6 | Why is CoAP ideal for constrained devices? | Minimal overhead, small code footprint, UDP-based (no connection state), and supports sleepy devices with low power consumption |
| 7 | What is the CoAP communication model? | Request-response model similar to HTTP, with GET, PUT, POST, DELETE methods, but optimized for constrained networks |
| 8 | What does AMQP stand for and what is its primary use case? | Advanced Message Queuing Protocol; designed for enterprise messaging with guaranteed delivery, transactions, and complex routing |
| 9 | What security protocol secures MQTT over TCP? | TLS (Transport Layer Security), creating MQTTS on port 8883 |
| 10 | What security protocol is used with CoAP over UDP? | DTLS (Datagram Transport Layer Security), adapted for connectionless UDP |
| 11 | What is a key tradeoff when choosing CoAP over MQTT? | CoAP has lower overhead and better suits constrained devices, but lacks built-in reliable delivery and pub-sub semantics that MQTT provides |
| 12 | Which protocol would you select for battery-powered sensors sending data over lossy networks? | CoAP, due to its lightweight UDP transport, low power requirements, and tolerance for constrained environments |
| 13 | What MQTT QoS level ensures a message is delivered exactly once? | QoS 2, using a four-step handshake (PUBLISH, PUBREC, PUBREL, PUBCOMP) |
| 14 | Why might AMQP be chosen over MQTT for an IoT backend? | AMQP offers enterprise features like message queuing, transactions, complex routing, and stronger delivery guarantees needed for business-critical systems |
| 15 | How does protocol choice fundamentally shape IoT architecture? | It determines reliability guarantees, power consumption, network overhead, scalability patterns, and security implementation approaches |

### Key Terms

| Term | Definition |
|------|-----------|
| MQTT | Message Queuing Telemetry Transport; lightweight publish-subscribe protocol over TCP designed for low-bandwidth, high-latency networks |
| CoAP | Constrained Application Protocol; RESTful protocol using UDP designed for resource-constrained IoT devices |
| AMQP | Advanced Message Queuing Protocol; enterprise-grade messaging protocol with guaranteed delivery and complex routing |
| QoS (Quality of Service) | Levels defining delivery guarantees between sender and receiver (reliability vs. overhead tradeoff) |
| TLS | Transport Layer Security; cryptographic protocol providing secure communication over TCP connections |
| DTLS | Datagram TLS; security protocol providing TLS-equivalent protection for UDP-based communications |
| Publish-Subscribe | Messaging pattern where senders (publishers) don't send to specific receivers; subscribers express interest in topics |

### Memory Tricks
- **MQTT = "Messages Queue via TCP Transport"** — remember it uses TCP and queues messages through a broker
- **CoAP is "Compact over Anything Protocol"** — think small, constrained, UDP-light like a tiny cap on a bottle
- **QoS levels 0-1-2 = "Zero guarantees, One acknowledgment, Two-way handshake"** — numbers match the delivery assurance steps
- **TLS for TCP, DTLS for Datagrams** — the D in DTLS matches the D in UDP (Datagram)
- **"AMQP = Always Message Queue Properly"** — enterprise systems demand proper, guaranteed message handling