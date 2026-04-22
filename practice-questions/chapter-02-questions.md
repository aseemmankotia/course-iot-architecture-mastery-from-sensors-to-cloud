## Chapter 2: Speaking the Right Language — Practice Questions

### Multiple Choice

**Q1.** In MQTT, which Quality of Service (QoS) level guarantees that a message is delivered exactly once to the subscriber?

A) QoS 0
B) QoS 1
C) QoS 2
D) QoS 3

<details>
<summary>Answer</summary>

**Correct: C**

QoS 2 provides exactly-once delivery through a four-part handshake (PUBLISH, PUBREC, PUBREL, PUBCOMP). QoS 0 offers at-most-once delivery with no acknowledgment. QoS 1 provides at-least-once delivery, which may result in duplicates. QoS 3 does not exist in the MQTT specification.

</details>

---

**Q2.** What transport layer protocol does CoAP use, and why is this significant for IoT devices?

A) TCP, because it provides reliable delivery
B) UDP, because it reduces overhead for constrained devices
C) SCTP, because it supports multi-homing
D) QUIC, because it combines reliability with low latency

<details>
<summary>Answer</summary>

**Correct: B**

CoAP uses UDP to minimize protocol overhead, making it ideal for constrained devices with limited memory, processing power, and battery life. TCP's connection establishment and maintenance overhead would be too costly for many IoT scenarios. SCTP and QUIC are not used by CoAP.

</details>

---

**Q3.** Which protocol would be most appropriate for an enterprise system requiring complex routing, message queuing, and transaction support between IoT gateways and backend systems?

A) MQTT
B) CoAP
C) AMQP
D) HTTP

<details>
<summary>Answer</summary>

**Correct: C**

AMQP (Advanced Message Queuing Protocol) is designed for enterprise messaging with features like message queuing, flexible routing, transactions, and security. MQTT is lightweight but lacks advanced routing. CoAP is designed for constrained devices. HTTP lacks native queuing and pub/sub capabilities.

</details>

---

**Q4.** What is the primary difference between TLS and DTLS in IoT security implementations?

A) TLS encrypts data while DTLS only authenticates
B) TLS works over TCP while DTLS works over UDP
C) DTLS provides stronger encryption than TLS
D) TLS is for cloud systems while DTLS is for edge devices

<details>
<summary>Answer</summary>

**Correct: B**

TLS (Transport Layer Security) operates over TCP connections, while DTLS (Datagram TLS) is designed for UDP-based protocols. DTLS handles the challenges of unreliable transport, including packet reordering and loss. Both provide similar levels of encryption and authentication; the difference is the underlying transport protocol they secure.

</details>

---

**Q5.** In MQTT, what happens when a client publishes a message with QoS 1 and the broker does not receive the PUBACK?

A) The message is discarded and marked as failed
B) The publisher retransmits the message
C) The broker requests a new connection
D) The subscriber is notified of delivery failure

<details>
<summary>Answer</summary>

**Correct: B**

With QoS 1, the publisher waits for a PUBACK acknowledgment from the broker. If the PUBACK is not received within a timeout period, the publisher retransmits the message with the DUP flag set. This ensures at-least-once delivery, though it may result in duplicate messages being received.

</details>

---

### True / False

**Q6.** CoAP's observe option allows a client to receive ongoing notifications from a server without repeatedly polling for updates. — **True / False**

<details>
<summary>Answer</summary>

**True**

The observe option in CoAP enables a publish-subscribe pattern where a client registers interest in a resource, and the server sends notifications whenever the resource state changes. This eliminates the need for continuous polling, saving bandwidth and power on constrained devices.

</details>

---

**Q7.** MQTT requires significantly more bandwidth than AMQP because of its verbose message headers. — **True / False**

<details>
<summary>Answer</summary>

**False**

MQTT is designed to be lightweight with minimal packet overhead—a fixed header can be as small as 2 bytes. AMQP, while feature-rich, has more complex framing and larger headers. MQTT was specifically created for low-bandwidth, high-latency networks where efficiency is critical.

</details>

---

### Short Answer

**Q8.** Explain the concept of "retained messages" in MQTT and describe a practical IoT use case where this feature would be valuable.

<details>
<summary>Answer</summary>

A retained message in MQTT is stored by the broker and immediately delivered to any new subscriber on that topic. The broker keeps only the last retained message per topic.

**Practical use case:** A temperature sensor publishes the current room temperature as a retained message. When a new monitoring dashboard connects and subscribes, it immediately receives the last known temperature instead of waiting for the next sensor reading. This is especially valuable for sensors that publish infrequently, ensuring new clients have immediate access to current state information.

</details>

---

**Q9.** List three key criteria you would consider when selecting between MQTT and CoAP for an IoT deployment, and briefly explain each.

<details>
<summary>Answer</summary>

1. **Network reliability and type:** MQTT uses TCP (reliable, connection-oriented), while CoAP uses UDP (unreliable, connectionless). Choose MQTT for networks where packet delivery is critical; choose CoAP for lossy networks where lightweight retransmission is preferred.

2. **Device constraints:** CoAP has lower overhead and is better suited for extremely constrained devices with limited RAM and processing power. MQTT, while lightweight, requires TCP stack overhead.

3. **Communication pattern:** MQTT excels at publish-subscribe patterns with many-to-many communication through a broker. CoAP follows a request-response model similar to HTTP, better suited for RESTful resource access and one-to-one communication.

</details>

---

### Scenario-Based

**Q10.** You are designing an IoT system for a remote agricultural monitoring network. The system includes 500 soil moisture sensors deployed across a large farm with intermittent cellular connectivity. Sensors need to report readings every 15 minutes, and farmers need real-time alerts when moisture drops below critical thresholds. The devices run on solar power with battery backup.

Recommend an appropriate communication protocol stack and justify your choices, addressing: (a) the primary data reporting protocol, (b) the QoS level or reliability mechanism, and (c) the security approach.

<details>
<summary>Answer</summary>

**(a) Primary Protocol: MQTT**

MQTT is ideal for this scenario because:
- Its publish-subscribe model efficiently handles 500 sensors publishing to a central broker
- Low bandwidth overhead conserves cellular data usage
- Broker-based architecture handles intermittent connectivity gracefully through persistent sessions
- Native support for real-time alerts through topic-based subscriptions

**(b) QoS Level: QoS 1 (at-least-once)**

QoS 1 provides a good balance:
- Ensures moisture readings are delivered even with connectivity issues
- Lower overhead than QoS 2, preserving battery life
- Duplicate readings can be handled application-side with timestamps
- For critical alerts, QoS 2 could be used selectively to guarantee exactly-once delivery

**(c) Security: TLS 1.3 with client certificates**

- TLS encrypts data in transit, protecting agricultural data
- Client certificates authenticate each sensor, preventing unauthorized devices
- Pre-shared keys could be considered as a lighter alternative if device constraints require it
- Session resumption reduces handshake overhead on reconnection

Alternative consideration: For extremely constrained sensors, CoAP with DTLS could reduce overhead further, with an MQTT gateway at the farm edge handling cloud communication.

</details>