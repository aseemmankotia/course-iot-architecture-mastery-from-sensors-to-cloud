## Chapter 3: Where Intelligence Lives — Practice Questions

### Multiple Choice

**Q1.** In the edge-fog-cloud computing hierarchy, which layer typically aggregates data from multiple edge devices before sending it to the cloud?

A) Edge layer
B) Fog layer
C) Cloud layer
D) Sensor layer

<details>
<summary>Answer</summary>

**Correct: B**

The fog layer sits between edge devices and the cloud, serving as an intermediate aggregation point. It collects data from multiple edge nodes, performs regional processing, and reduces the volume of data transmitted to the cloud. Edge devices handle immediate local processing, while the cloud provides centralized storage and heavy computation.

</details>

---

**Q2.** A manufacturing plant needs to detect equipment failures within 5 milliseconds to trigger emergency shutoffs. Which computing approach is most appropriate?

A) Cloud computing with high-bandwidth connections
B) Edge computing with local processing
C) Fog computing at the regional data center
D) Hybrid cloud with content delivery networks

<details>
<summary>Answer</summary>

**Correct: B**

Edge computing with local processing is essential for ultra-low latency requirements like 5ms response times. Network round-trips to fog or cloud layers would introduce unacceptable delays. Edge devices can process sensor data and trigger actions locally without network dependency, meeting strict real-time requirements.

</details>

---

**Q3.** Which function is NOT typically a responsibility of an edge gateway?

A) Protocol translation between devices and upstream systems
B) Long-term historical data archiving
C) Local data filtering and aggregation
D) Device authentication and security enforcement

<details>
<summary>Answer</summary>

**Correct: B**

Long-term historical data archiving is typically handled by cloud infrastructure, which offers scalable, cost-effective storage. Edge gateways focus on real-time functions: protocol translation, data filtering, aggregation, security enforcement, and temporary buffering. Their limited storage capacity makes them unsuitable for archival purposes.

</details>

---

**Q4.** What is the primary benefit of data filtering at the edge?

A) Increased data accuracy through cloud-based validation
B) Reduced bandwidth consumption and transmission costs
C) Improved data encryption strength
D) Enhanced device discovery capabilities

<details>
<summary>Answer</summary>

**Correct: B**

Data filtering at the edge eliminates redundant, irrelevant, or unchanged data before transmission, significantly reducing bandwidth consumption and associated costs. Rather than sending every sensor reading, edge filtering transmits only meaningful changes or aggregated summaries, making efficient use of network resources.

</details>

---

**Q5.** Which latency reduction strategy involves predicting and pre-loading data before it's requested?

A) Connection pooling
B) Data compression
C) Prefetching and caching
D) Protocol optimization

<details>
<summary>Answer</summary>

**Correct: C**

Prefetching and caching anticipate future data needs and load information in advance, eliminating wait times when the data is actually requested. Connection pooling manages existing connections efficiently, compression reduces transmission time, and protocol optimization reduces overhead—but only prefetching proactively loads data before requests.

</details>

---

### True / False

**Q6.** Fog computing and edge computing are identical concepts, with the terms used interchangeably in industry literature. — **True / False**

<details>
<summary>Answer</summary>

**False**

While related, fog and edge computing are distinct paradigms. Edge computing occurs directly on or very near the source devices, handling immediate local processing. Fog computing represents an intermediate layer between edge and cloud, providing regional aggregation, additional processing power, and coordination across multiple edge nodes. Fog nodes typically have more resources than edge devices but less than cloud data centers.

</details>

---

**Q7.** Store-and-forward is an offline resilience pattern where edge devices buffer data locally when cloud connectivity is lost and transmit it once connectivity is restored. — **True / False**

<details>
<summary>Answer</summary>

**True**

Store-and-forward is a fundamental offline resilience pattern. When network connectivity fails, edge devices continue collecting data and storing it in local buffers. Once connectivity resumes, the buffered data is forwarded to upstream systems, ensuring no data loss during outages. This pattern requires careful management of local storage capacity and data prioritization.

</details>

---

### Short Answer

**Q8.** Describe three key responsibilities of an edge gateway in an IoT architecture.

<details>
<summary>Answer</summary>

Edge gateways typically handle: (1) **Protocol translation** — converting between device protocols (like Modbus, BLE, or Zigbee) and standard IP-based protocols for upstream communication; (2) **Data aggregation and filtering** — combining data from multiple sensors, removing duplicates, and reducing data volume before transmission; (3) **Security enforcement** — authenticating devices, encrypting data, and serving as a security boundary between field devices and external networks. Additional responsibilities may include local processing, temporary data storage, and device management.

</details>

---

**Q9.** Explain the difference between latency and bandwidth, and why edge computing primarily addresses latency concerns.

<details>
<summary>Answer</summary>

**Latency** is the time delay for data to travel between two points, while **bandwidth** is the volume of data that can be transmitted per unit of time. Edge computing primarily addresses latency because it processes data physically closer to where it's generated, eliminating network round-trip delays to distant cloud servers. Even with unlimited bandwidth, data still takes time to travel long distances. For time-critical applications requiring millisecond responses, local edge processing is essential regardless of available bandwidth.

</details>

---

### Scenario-Based

**Q10.** A smart agriculture company deploys 500 soil moisture sensors across a remote farm with unreliable cellular connectivity. The sensors generate readings every 30 seconds, but farmers only need alerts when moisture drops below critical thresholds. The company wants to minimize data costs while ensuring no critical alerts are missed during connectivity outages. Design an architecture that addresses these requirements, specifying where intelligence should be placed and what offline resilience patterns to implement.

<details>
<summary>Answer</summary>

**Recommended Architecture:**

**Edge Layer:** Deploy edge gateways (one per field section) that receive raw sensor data via low-power protocols like LoRaWAN. Each gateway performs:
- Local threshold analysis to detect critical moisture levels
- Data filtering to transmit only significant changes (not every 30-second reading)
- Aggregation to send hourly summaries during normal conditions

**Offline Resilience Patterns:**
- **Store-and-forward:** Gateways buffer all readings locally when cellular connectivity fails, transmitting when restored
- **Priority queuing:** Critical alerts get transmission priority over routine data when connectivity resumes
- **Local alerting:** Gateways trigger local alarms (sirens, lights) for critical conditions even without cloud connectivity
- **Graceful degradation:** Continue local monitoring and alerting indefinitely; sync with cloud when possible

**Fog/Cloud Layer:** Regional fog nodes aggregate data from multiple farms for trend analysis, while the cloud handles long-term storage, analytics dashboards, and farmer mobile notifications.

This design minimizes cellular data costs through filtering, ensures critical alerts via local processing, and prevents data loss through buffering.

</details>