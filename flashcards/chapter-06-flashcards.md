## Chapter 6: Commanding Your Fleet — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | What is a device twin (or device shadow)? | A cloud-based JSON document that stores device state information, including metadata, configurations, and conditions, serving as a virtual representation of a physical device. |
| 2 | What are the two main property types in a device twin? | **Desired properties** (set by cloud/backend to indicate intended state) and **Reported properties** (sent by device to indicate actual current state). |
| 3 | Why do device twins decouple cloud intent from device reality? | They allow the cloud to set desired states without requiring the device to be online, and devices can report actual states independently, enabling asynchronous state management. |
| 4 | What is eventual consistency in IoT? | A consistency model where the system guarantees that if no new updates are made, all nodes will eventually reflect the same state, accepting temporary inconsistencies. |
| 5 | When would you use a direct method instead of desired properties? | For synchronous, real-time commands requiring immediate response (like reboot, emergency stop, or diagnostic requests) where you need confirmation of execution. |
| 6 | What happens to desired properties when a device is offline? | They persist in the cloud twin; when the device reconnects, it receives the latest desired state and can converge to match it. |
| 7 | What is offline device state convergence? | The process by which a device, upon reconnecting, synchronizes its state with the cloud twin by reading desired properties and updating reported properties. |
| 8 | How does a device know its state is out of sync with the cloud? | By comparing reported properties with desired properties in the twin; any delta indicates a state mismatch requiring action. |
| 9 | What is the key design principle for IoT command systems? | Design for convergence, not synchronous calls—assume devices will be offline and build systems that gracefully handle state reconciliation. |
| 10 | What metadata might a device twin contain beyond desired/reported properties? | Device ID, connection state, last activity time, firmware version, tags for grouping, and ETags for concurrency control. |
| 11 | Why are direct methods considered "synchronous" in IoT? | They establish a request-response pattern where the cloud waits for the device to execute the command and return a result within a timeout period. |
| 12 | What problem does the device twin pattern solve for fleet management? | Enables managing thousands of devices at scale without requiring all devices to be simultaneously online or individually addressed. |
| 13 | How should you handle conflicting states between desired and reported properties? | Implement reconciliation logic on the device that prioritizes desired state, applies changes safely, then updates reported properties to confirm. |
| 14 | What is a common pitfall when using direct methods for IoT commands? | Over-relying on them for configuration changes, which fails when devices are offline—use desired properties for persistent configuration instead. |
| 15 | How do device twins enable "intent-based" fleet management? | Operators declare the desired end state; the system handles getting each device there regardless of current state or connectivity. |

### Key Terms

| Term | Definition |
|------|-----------|
| Device Twin/Shadow | A cloud-stored JSON representation of a device's state, enabling bidirectional synchronization between cloud and device. |
| Desired Properties | Cloud-set properties expressing the intended configuration or state the device should achieve. |
| Reported Properties | Device-sent properties reflecting the actual current state of the physical device. |
| Direct Methods | Synchronous cloud-to-device commands that require immediate execution and response from an online device. |
| Eventual Consistency | A model accepting temporary state mismatches with guarantee of eventual synchronization across all components. |
| State Convergence | The process of a device aligning its actual state with the cloud-specified desired state after reconnection. |
| Fleet Management | The practice of monitoring, configuring, and commanding large numbers of IoT devices as a coordinated group. |

### Memory Tricks
- **D.R.I.V.E.**: **D**esired (cloud wants), **R**eported (device has), **I**ntent vs reality, **V**irtual twin, **E**ventual sync
- Think of device twins like a **pizza order**: Desired = what you ordered, Reported = what arrived. The restaurant (device) works to make them match!
- **Direct methods = phone call** (need them to answer now), **Desired properties = voicemail** (they'll get it when they can)