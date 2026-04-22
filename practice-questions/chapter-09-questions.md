## Chapter 9: Seeing Everything Clearly — Practice Questions

### Multiple Choice

**Q1.** When managing a fleet of 50,000 IoT devices, which approach best balances visibility with resource constraints?

A) Store complete debug logs from every device in real-time
B) Use fleet-level aggregated metrics with selective per-device logging triggered by anomalies
C) Disable logging entirely to reduce bandwidth costs
D) Log only from a random 1% sample of devices permanently

<details>
<summary>Answer</summary>

**Correct: B**

Fleet-level metrics provide broad visibility into system health while consuming minimal resources. Selective per-device logging activated during anomalies captures detailed diagnostics only when needed. Option A is cost-prohibitive at scale. Option C eliminates critical diagnostic capability. Option D misses issues affecting the majority of devices.

</details>

---

**Q2.** In distributed tracing across the device-cloud boundary, what is the primary challenge that distinguishes IoT from traditional microservices tracing?

A) IoT devices cannot generate unique trace IDs
B) Network latency between devices and cloud introduces timing gaps and potential trace discontinuity
C) Cloud services don't support correlation IDs
D) Tracing adds too much CPU overhead for any embedded device

<details>
<summary>Answer</summary>

**Correct: B**

IoT devices often operate on unreliable networks with intermittent connectivity, causing traces to fragment. Messages may be queued locally for hours before transmission, creating timing discontinuities. Modern embedded devices can generate trace IDs (A is false), cloud services fully support correlation (C is false), and lightweight tracing implementations exist for resource-constrained devices (D is overstated).

</details>

---

**Q3.** A device is reporting telemetry every 5 minutes as expected, but its sensor readings have been stuck at the same value for 3 days. According to the "five meanings of offline," which category does this represent?

A) Network offline
B) Functionally offline
C) Power offline
D) Cloud offline

<details>
<summary>Answer</summary>

**Correct: B**

The device is functionally offline—it maintains connectivity and sends data, but the core function (sensing) has failed. Network offline would mean no communication. Power offline means the device is unpowered. Cloud offline refers to the backend being unavailable. Functionally offline is particularly insidious because standard heartbeat monitoring misses it.

</details>

---

**Q4.** Which anomaly detection approach is most appropriate for detecting gradual sensor drift in operational metrics?

A) Static thresholds set during initial deployment
B) Simple rate-of-change alerts over 1-minute windows
C) Baseline comparison using rolling historical averages with seasonal adjustment
D) Manual review of daily reports

<details>
<summary>Answer</summary>

**Correct: C**

Gradual drift occurs slowly over weeks or months, making static thresholds ineffective until the problem becomes severe. Short-window rate-of-change misses slow drift. Manual review doesn't scale. Rolling historical baselines with seasonal adjustment detect subtle deviations from expected patterns while accounting for legitimate variations like temperature cycles.

</details>

---

**Q5.** For a fleet of agricultural sensors deployed across remote regions, which log retention strategy optimizes cost while maintaining diagnostic capability?

A) Keep all logs at full resolution indefinitely in hot storage
B) Implement tiered retention: 7 days hot, 30 days warm with aggregation, 1 year cold with samples only
C) Delete all logs after 24 hours to minimize storage costs
D) Store only error logs, discarding all informational and debug entries

<details>
<summary>Answer</summary>

**Correct: B**

Tiered retention balances cost with diagnostic needs. Recent logs need full resolution for active troubleshooting. Aggregated warm storage supports trend analysis. Cold storage with statistical samples enables long-term pattern analysis. Option A is prohibitively expensive. Option C loses critical diagnostic history. Option D misses context needed to diagnose errors.

</details>

---

### True / False

**Q6.** A device that successfully sends heartbeat messages every 60 seconds can be definitively classified as fully operational. — **True / False**

**False**

*Heartbeats confirm network connectivity and basic device responsiveness but reveal nothing about functional health. A device could have a failed sensor, corrupted firmware logic, or degraded actuators while still maintaining heartbeat communication. This is why observability must include functional health indicators beyond simple connectivity checks.*

---

**Q7.** Distributed tracing in IoT systems should use the same trace ID from sensor reading through cloud processing to enable end-to-end correlation. — **True / False**

**True**

*Maintaining a consistent trace ID (or correlation ID) across the entire data path—from device sensor through edge processing, network transmission, and cloud pipeline—enables operators to reconstruct the complete journey of any data point. This is essential for debugging latency issues, identifying where data transformations occur, and troubleshooting processing failures.*

---

### Short Answer

**Q8.** List and briefly explain the "five meanings of offline" in IoT fleet management.

<details>
<summary>Answer</summary>

1. **Power offline** — Device has no electrical power (battery dead, unplugged, power failure)
2. **Network offline** — Device is powered but cannot reach the network (connectivity issues, router failure)
3. **Cloud offline** — Device connects to network but cannot reach cloud services (backend outage, certificate expiration)
4. **Application offline** — Device reaches cloud but application layer has failed (crashed process, corrupted state)
5. **Functionally offline** — Device appears healthy but core function has failed (broken sensor, stuck actuator)

Each requires different detection mechanisms and remediation approaches.

</details>

---

**Q9.** Explain why fleet-level metrics and per-device logging serve different purposes and how they complement each other.

<details>
<summary>Answer</summary>

**Fleet-level metrics** provide aggregate visibility into system health—average latency, error rates, connectivity percentages across thousands of devices. They answer "is the system healthy?" and highlight trends affecting multiple devices. They're cost-effective because they compress data from many devices into statistical summaries.

**Per-device logging** provides granular detail for individual device troubleshooting—specific error messages, state transitions, sensor readings. They answer "why is this specific device misbehaving?"

They complement each other through a drill-down workflow: fleet metrics identify anomalies (e.g., "error rate spiked 3% in region X"), then per-device logs diagnose root causes for affected devices. This layered approach provides both breadth and depth while managing costs.

</details>

---

### Scenario-Based

**Q10.** Your company operates 25,000 smart water meters across a metropolitan area. Over the past week, your monitoring dashboard shows that reported water usage across the fleet has dropped 15% compared to the same period last year, despite no known changes in customer behavior. The device connectivity rate remains at 99.2% (normal), and error logs show no increase in failures.

Describe your systematic approach to investigating this anomaly, including which observability tools and techniques from this chapter you would apply, and what the "five meanings of offline" framework suggests about possible causes.

<details>
<summary>Answer</summary>

**Investigation Approach:**

1. **Apply the functional offline lens** — Despite high connectivity, meters may be functionally compromised. The "five meanings" framework suggests checking whether devices are properly measuring flow, not just communicating.

2. **Segment fleet-level metrics** — Break down the 15% drop by geography, device firmware version, installation date, and meter model. Look for clusters that explain the anomaly (e.g., "all firmware v2.3 devices show 25% drop").

3. **Use anomaly detection on operational metrics** — Compare individual device baselines. Identify which devices show readings significantly below their historical patterns vs. legitimate low usage.

4. **Enable selective per-device logging** — For devices with suspicious readings, activate detailed logging to capture sensor raw values, calibration data, and processing logic outputs.

5. **Implement distributed tracing** — Trace the data path from sensor → device processing → transmission → cloud ingestion to identify where values might be getting truncated or miscalculated.

6. **Check for correlated events** — Review what changed: firmware updates, backend processing changes, seasonal adjustments, or meter recalibrations.

**Likely causes to investigate:**
- Sensor degradation (functional offline) affecting a batch of meters
- Firmware bug introduced in recent update
- Backend calculation change
- Physical issues (e.g., debris partially blocking flow sensors)

**Cost-effective approach:** Start with fleet-level segmentation before enabling expensive per-device diagnostics.

</details>