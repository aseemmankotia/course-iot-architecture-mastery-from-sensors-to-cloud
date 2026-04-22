## Chapter 6: Commanding Your Fleet — Practice Questions

### Multiple Choice

**Q1.** In the device twin pattern, what is the primary purpose of "desired properties"?

A) To store historical telemetry data from the device
B) To represent the target configuration that the cloud wants the device to achieve
C) To record the current firmware version installed on the device
D) To track the device's network connectivity status

<details>
<summary>Answer</summary>

**Correct: B**

Desired properties represent the target state that the cloud or backend application wants the device to achieve. Option A is incorrect because telemetry data is handled separately from device twins. Option C describes reported properties, not desired properties. Option D relates to device connectivity monitoring, which is a separate concern from configuration management.

</details>

---

**Q2.** When a device has been offline and reconnects to the cloud, what happens with conflicting desired and reported states?

A) The cloud state always overwrites the device state completely
B) The device state always takes priority over cloud commands
C) The device receives pending desired state changes and reconciles toward the target configuration
D) Both states are deleted and must be manually reconfigured

<details>
<summary>Answer</summary>

**Correct: C**

When a device reconnects, the device twin mechanism delivers any pending desired state changes that occurred while offline. The device then processes these changes and updates its reported properties accordingly, achieving eventual consistency. Options A and B are incorrect because neither side has absolute priority—reconciliation occurs. Option D is incorrect as the twin pattern specifically handles offline scenarios gracefully.

</details>

---

**Q3.** Which communication pattern is most appropriate when you need an immediate response from a device, such as triggering an emergency shutdown?

A) Updating desired properties in the device twin
B) Publishing a message to a telemetry topic
C) Using direct methods (synchronous commands)
D) Modifying reported properties from the cloud

<details>
<summary>Answer</summary>

**Correct: C**

Direct methods provide synchronous, request-response communication ideal for time-sensitive commands requiring immediate acknowledgment. Option A uses desired properties which are asynchronous and don't guarantee immediate execution. Option B is for device-to-cloud communication, not commands. Option D is incorrect because the cloud cannot directly modify reported properties—only the device can update them.

</details>

---

**Q4.** What is "eventual consistency" in the context of IoT device management?

A) A guarantee that all devices will have identical configurations within milliseconds
B) A model where device state will converge with desired state over time, even with network interruptions
C) A requirement that devices must be online continuously to receive updates
D) A protocol that prevents any state changes during network partitions

<details>
<summary>Answer</summary>

**Correct: B**

Eventual consistency acknowledges that in distributed IoT systems, state synchronization happens over time rather than instantaneously. The system guarantees that given enough time without new updates, all devices will eventually reach the desired state. Option A describes strong consistency, which is impractical in IoT. Option C contradicts the offline-capable nature of twin patterns. Option D describes a blocking approach that would make IoT systems impractical.

</details>

---

**Q5.** A device twin shows desired properties with `"firmwareVersion": "2.1.0"` and reported properties with `"firmwareVersion": "2.0.5"`. What does this indicate?

A) The device has a newer firmware than requested
B) The device has not yet completed updating to the requested firmware version
C) There is a critical error in the device twin synchronization
D) The firmware update was rejected by the device

<details>
<summary>Answer</summary>

**Correct: B**

The discrepancy between desired (2.1.0) and reported (2.0.5) properties indicates the device either hasn't received the update command, is currently processing the update, or hasn't yet reported completion. This is normal behavior in eventually consistent systems. Option A is incorrect because 2.0.5 is older than 2.1.0. Option C is incorrect as this represents expected behavior during updates. Option D cannot be determined from version numbers alone—rejection would typically include error status in reported properties.

</details>

---

### True / False

**Q6.** Device twins can only store simple key-value pairs and cannot represent nested or hierarchical configuration structures. — **True / False**

**False.** Device twins support complex, nested JSON structures for both desired and reported properties. This allows representation of sophisticated device configurations including nested objects, arrays, and hierarchical settings. For example, a twin could contain nested properties like `{ "settings": { "display": { "brightness": 80, "theme": "dark" } } }`.

---

**Q7.** Direct methods require the target device to be online at the time of invocation, whereas device twin updates can be queued for offline devices. — **True / False**

**True.** Direct methods are synchronous and require an active connection to the device—they will fail or timeout if the device is offline. In contrast, device twin desired property updates are persisted in the cloud and delivered to the device when it reconnects, making twins suitable for managing devices with intermittent connectivity.

---

### Short Answer

**Q8.** Explain why you might use both desired/reported properties AND direct methods in the same IoT solution. Provide an example scenario.

<details>
<summary>Answer</summary>

Different communication patterns serve different purposes in IoT architectures. Desired/reported properties are ideal for configuration management and state synchronization because they persist across connectivity gaps and provide eventual consistency. Direct methods are better for immediate, time-sensitive operations requiring confirmation.

Example scenario: A smart thermostat system might use desired properties to set the target temperature schedule (configuration that can sync whenever the device connects) while using direct methods to trigger an immediate "boost heat" command when the homeowner is arriving early and needs instant feedback that the command was received and executed.

</details>

---

**Q9.** What challenges arise when managing eventual consistency across a fleet of thousands of devices, and how might you address them?

<details>
<summary>Answer</summary>

Key challenges include:

1. **Visibility into fleet state**: With thousands of devices at different stages of convergence, tracking overall compliance is difficult. Address this with dashboards aggregating reported properties and calculating compliance percentages.

2. **Stale device detection**: Some devices may never converge due to hardware failures or permanent disconnection. Implement timeouts and alerting for devices that haven't reported within expected intervals.

3. **Rollout coordination**: Pushing updates to all devices simultaneously may overwhelm infrastructure or cause fleet-wide issues. Use staged rollouts with canary deployments and automatic rollback triggers.

4. **Conflict resolution**: Multiple desired state changes while devices are offline need clear versioning or timestamp-based resolution strategies to ensure deterministic outcomes.

</details>

---

### Scenario-Based

**Q10.** You are designing a fleet management system for 50,000 industrial sensors deployed in remote locations with unreliable cellular connectivity. Sensors typically connect for 5 minutes every hour to conserve battery. The operations team needs to: (a) update sensor sampling rates, (b) retrieve current battery levels, and (c) trigger immediate diagnostic tests when troubleshooting specific sensors. Design the communication architecture explaining which pattern you would use for each requirement and why.

<details>
<summary>Answer</summary>

**Architecture Design:**

**(a) Update sensor sampling rates — Use Desired Properties**

Sampling rate changes are configuration updates that don't require immediate execution. Using desired properties in device twins ensures that even when sensors are offline, the new configuration is persisted in the cloud. When each sensor connects during its 5-minute window, it retrieves the updated desired properties and applies the new sampling rate. The sensor then updates reported properties to confirm the change, allowing the operations team to track rollout progress across the fleet.

**(b) Retrieve current battery levels — Use Reported Properties**

Battery level is device state that should be pushed from the device during each connection window. Configure sensors to update reported properties with current battery level on each connection. The operations team can query device twins at any time to see the last-reported battery level along with the timestamp, without requiring the device to be online. For fleet-wide analysis, aggregate queries across all twins can identify sensors needing battery replacement.

**(c) Trigger immediate diagnostic tests — Use Direct Methods with Fallback**

For troubleshooting specific sensors, direct methods provide synchronous command execution with immediate feedback. However, given the intermittent connectivity, implement a hybrid approach: attempt a direct method first, and if the device is offline, fall back to setting a desired property flag like `"runDiagnostic": true`. When the sensor next connects, it checks for this flag, runs diagnostics, and reports results. Include notification logic to alert the operations team when diagnostic results arrive, since the response may be delayed up to an hour.

**Additional Considerations:**
- Implement message queuing with TTL (time-to-live) appropriate for the connection interval
- Use version tags on desired properties to handle multiple configuration changes between connections
- Design reported properties to include last-sync timestamps for staleness detection

</details>