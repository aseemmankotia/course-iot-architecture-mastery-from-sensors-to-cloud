## Chapter 8: When Things Go Dark — Practice Questions

### Multiple Choice

**Q1.** What is the primary purpose of adding jitter to exponential backoff algorithms in IoT reconnection scenarios?

A) To increase the speed of reconnection attempts
B) To prevent synchronized reconnection attempts from multiple devices
C) To reduce power consumption during offline periods
D) To improve data compression during transmission

<details>
<summary>Answer</summary>

**Correct: B**

Adding jitter introduces randomness to the backoff timing, which prevents a "thundering herd" problem where thousands of devices attempt to reconnect simultaneously after an outage. Option A is incorrect because jitter actually delays some reconnections. Option C is unrelated to jitter's purpose. Option D has nothing to do with backoff algorithms.

</details>

---

**Q2.** Which device-side buffering strategy is most appropriate for a sensor that generates high-frequency, time-series data with limited storage capacity?

A) Store all data indefinitely until connection is restored
B) Circular buffer with oldest data overwritten first
C) Priority queue based on alphabetical ordering
D) Random sampling with no buffer structure

<details>
<summary>Answer</summary>

**Correct: B**

A circular buffer efficiently manages limited storage by continuously overwriting the oldest data, ensuring the most recent readings are preserved. Option A is impractical with limited storage. Option C's alphabetical ordering has no relevance to time-series data importance. Option D would lose data unpredictably and break time-series continuity.

</details>

---

**Q3.** What makes a command idempotent in IoT systems?

A) It can only be executed once per device lifecycle
B) Executing it multiple times produces the same result as executing it once
C) It requires acknowledgment before execution
D) It automatically expires after a timeout period

<details>
<summary>Answer</summary>

**Correct: B**

Idempotent commands produce identical outcomes regardless of how many times they're executed, which is crucial when network issues may cause duplicate deliveries. Option A describes a one-time command, not idempotency. Option C describes acknowledged delivery. Option D describes command expiration, which is a separate concern.

</details>

---

**Q4.** Which message deduplication pattern relies on the sender generating a unique value for each message?

A) Timestamp-based deduplication
B) Content hash deduplication
C) Client-generated message ID deduplication
D) Server-assigned sequence numbers

<details>
<summary>Answer</summary>

**Correct: C**

Client-generated message IDs (such as UUIDs) are created by the sender and allow receivers to identify and discard duplicates. Option A can fail with clock drift or identical timestamps. Option B may incorrectly deduplicate legitimately identical messages. Option D requires server involvement and doesn't rely on the sender.

</details>

---

**Q5.** In a reconnection storm mitigation strategy, what is the recommended approach when a backend service returns a 503 Service Unavailable response?

A) Immediately retry the connection
B) Stop all reconnection attempts permanently
C) Respect the Retry-After header and apply backoff
D) Switch to a different communication protocol

<details>
<summary>Answer</summary>

**Correct: C**

A 503 response indicates temporary unavailability, and the Retry-After header provides guidance on when to attempt reconnection. Combining this with backoff prevents overwhelming the recovering service. Option A would worsen the storm. Option B is overly aggressive and would leave devices offline. Option D doesn't address the underlying capacity issue.

</details>

---

### True / False

**Q6.** Exponential backoff algorithms should have no upper limit on the maximum delay between retry attempts. — **True / False**

**False**

*Exponential backoff should always include a maximum cap (ceiling) on the delay. Without a cap, devices could wait unreasonably long periods (hours or days) between attempts, making recovery unacceptably slow. A typical ceiling might be 5-30 minutes depending on the use case.*

---

**Q7.** Message deduplication windows should be sized based on the maximum expected network partition duration plus processing time. — **True / False**

**True**

*The deduplication window must be large enough to catch duplicates that may arrive after extended offline periods. If the window is too short, late-arriving duplicates from prolonged outages will be processed as new messages, causing unintended duplicate actions.*

---

### Short Answer

**Q8.** Explain why a "set temperature to 72°F" command is idempotent while an "increase temperature by 2°F" command is not.

<details>
<summary>Answer</summary>

The "set temperature to 72°F" command is idempotent because executing it once or multiple times results in the same final state—the temperature will be 72°F regardless of how many times the command is received. The "increase temperature by 2°F" command is not idempotent because each execution changes the state cumulatively. If this command is received three times due to network retries, the temperature increases by 6°F total instead of the intended 2°F, producing different and incorrect results.

</details>

---

**Q9.** Describe two criteria an IoT device might use to prioritize which buffered messages to transmit first when connectivity is restored.

<details>
<summary>Answer</summary>

1. **Temporal ordering**: Transmit oldest messages first to maintain chronological consistency for time-series data analysis, ensuring downstream systems receive events in the order they occurred.

2. **Message priority/severity**: Transmit critical alerts or anomaly detections before routine telemetry, ensuring important events (like safety alarms or threshold breaches) reach the cloud even if bandwidth or time is limited.

Other valid criteria include: data freshness (newest first for real-time dashboards), message size (smaller messages first to maximize throughput), or business value (high-value sensor data prioritized over diagnostic logs).

</details>

---

### Scenario-Based

**Q10.** A smart agriculture company deploys 10,000 soil moisture sensors across remote farmland. Due to a regional cellular outage lasting 4 hours, all sensors lose connectivity simultaneously. When service is restored, the backend servers crash repeatedly due to connection overload. The sensors use a simple fixed 5-second retry interval with no backoff. What specific changes would you recommend to prevent this situation, and how would you handle the buffered data accumulated during the outage?

<details>
<summary>Answer</summary>

**Reconnection Strategy Changes:**
1. Implement exponential backoff starting at 1-2 seconds, doubling with each failed attempt up to a maximum of 5-10 minutes
2. Add random jitter (±20-50% of the calculated delay) to spread reconnection attempts across time
3. Include a staggered initial delay based on device ID hash to prevent immediate synchronized reconnection
4. Respect server-provided Retry-After headers during overload conditions

**Buffered Data Handling:**
1. Implement a circular buffer on each device sized for the expected maximum outage duration (e.g., 6+ hours of readings)
2. Upon reconnection, use a throttled upload approach—don't transmit all buffered data immediately
3. Include message IDs and timestamps to enable server-side deduplication in case of partial transmission failures
4. Prioritize transmitting aggregate summaries or latest readings first, with historical data following during low-traffic periods
5. Consider server-side rate limiting per device to prevent any single device from overwhelming resources

These changes transform the reconnection from a simultaneous "thundering herd" into a gradual, distributed recovery that the backend can handle.

</details>