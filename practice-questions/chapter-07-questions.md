## Chapter 7: Updates That Don't Brick Your Fleet — Practice Questions

### Multiple Choice

**Q1.** In an A/B partition scheme, what is the primary purpose of maintaining two separate system partitions?

A) To double the available storage space for user data
B) To enable seamless updates with automatic rollback capability
C) To run two different operating systems simultaneously
D) To improve read/write performance through load balancing

<details>
<summary>Answer</summary>

**Correct: B**

The A/B partition scheme maintains two complete system partitions so that updates can be written to the inactive partition while the device continues running on the active one. If the new update fails to boot properly, the bootloader can automatically roll back to the previous working partition. Option A is incorrect because A/B actually reduces available storage. Option C is wrong as only one partition runs at a time. Option D is incorrect because partitions aren't used for load balancing.

</details>

---

**Q2.** During a canary deployment, 2% of your 50,000 device fleet receives a firmware update. After 48 hours, you observe a 15% failure rate in the canary group compared to 0.5% in the control group. What should happen next?

A) Proceed with full deployment since 85% of canary devices are working
B) Trigger an automatic rollback and halt the deployment
C) Increase the canary group to 10% for more data
D) Wait another 48 hours before making a decision

<details>
<summary>Answer</summary>

**Correct: B**

A 15% failure rate versus a 0.5% baseline represents a 30x increase in failures, which is a clear signal that the update has serious issues. Auto-rollback triggers should activate when failure rates exceed predefined thresholds. Option A ignores a critical failure signal. Option C would expose more devices to a problematic update. Option D unnecessarily delays action when the data is already conclusive.

</details>

---

**Q3.** Delta updates optimize bandwidth by transmitting only the differences between firmware versions. Which scenario would make delta updates LEAST effective?

A) Updating from version 2.1 to version 2.2 with minor bug fixes
B) Updating from version 1.0 to version 3.0 with a complete codebase rewrite
C) Updating configuration files that changed by 5%
D) Updating a fleet where all devices are on the same current version

<details>
<summary>Answer</summary>

**Correct: B**

Delta updates work by computing binary differences between versions. When the entire codebase has been rewritten, the delta may be nearly as large as (or larger than) a full image due to the computation overhead and lack of common bytes. Option A represents an ideal delta scenario with small changes. Option C also benefits greatly from delta compression. Option D describes uniform fleet state, which simplifies delta distribution but doesn't affect delta effectiveness.

</details>

---

**Q4.** Which convergence metric would best indicate that a staged rollout is ready to proceed to the next deployment ring?

A) 100% of devices in the current ring have downloaded the update
B) Average download speed across the fleet exceeds 1 Mbps
C) 95% of devices in the current ring report healthy status after 24 hours of operation
D) The update server has sufficient bandwidth for the next ring

<details>
<summary>Answer</summary>

**Correct: C**

Convergence metrics measure not just update delivery but successful operation post-update. Having 95% of devices report healthy status after a meaningful operational period indicates the update is stable and safe to expand. Option A only measures download completion, not update success. Option B measures infrastructure performance, not update quality. Option D is an operational concern but doesn't validate update stability.

</details>

---

**Q5.** A firmware image is signed using asymmetric cryptography. Where should the private signing key be stored?

A) Embedded in the device firmware for verification
B) In a Hardware Security Module (HSM) at the build server
C) On each IoT device's secure element
D) In the cloud deployment management console

<details>
<summary>Answer</summary>

**Correct: B**

The private signing key must be kept secure and should never leave a controlled environment. An HSM at the build server protects the key while allowing automated signing during the build process. Option A is critically wrong—devices need the public key for verification, not the private key. Option C would distribute the private key to thousands of devices, violating security principles. Option D exposes the key to potential cloud infrastructure compromises.

</details>

---

### True / False

**Q6.** A bootloader with rollback capability should automatically revert to the previous firmware version if the new version fails to boot three consecutive times. — **True / False**

**True**

*Implementing a boot attempt counter is a standard pattern for safe OTA updates. The bootloader tracks failed boot attempts, and after a configured threshold (commonly 3 attempts), it marks the current partition as invalid and switches back to the known-good previous version. This prevents devices from becoming permanently bricked due to a faulty update that crashes during boot.*

---

**Q7.** Ring-based deployment patterns require all devices in one ring to complete their updates before any device in the next ring can begin updating. — **True / False**

**False**

*Ring-based deployments require validation criteria to be met (convergence metrics, health checks, time-based observation periods) before proceeding to the next ring, but this doesn't mean 100% completion is required. Typically, you proceed when a sufficient percentage of devices in the current ring have successfully updated and remained healthy for a defined period. Some devices may be offline or slow to update, and waiting for 100% completion could delay deployments indefinitely.*

---

### Short Answer

**Q8.** Explain why firmware images should include version information that the bootloader can verify independently of the main application code.

<details>
<summary>Answer</summary>

The bootloader must verify version information independently to prevent downgrade attacks and ensure update integrity before executing potentially malicious code. If version checking relied on the application code itself, a compromised or corrupted firmware could report false version information. Independent verification in the bootloader ensures that: (1) rollback protection is enforced by rejecting older versions, (2) the correct partition is selected during boot, (3) update validation occurs before any untrusted code executes, and (4) the device can make safe boot decisions even if the main firmware is corrupted.

</details>

---

**Q9.** Describe two methods for optimizing OTA update bandwidth usage beyond delta updates.

<details>
<summary>Answer</summary>

Two additional methods for bandwidth optimization include:

1. **Compression**: Applying algorithms like LZ4, zstd, or LZMA to firmware images before transmission significantly reduces transfer size. The device decompresses the image after download. This works alongside delta updates for compounded savings.

2. **Update scheduling and throttling**: Distributing updates during off-peak network hours, implementing rate limiting per device, and staggering rollout timing prevents bandwidth congestion. This doesn't reduce total bytes transferred but optimizes network utilization and reduces peak load.

Other valid answers include: local caching/peer-to-peer distribution, resumable downloads to avoid retransmitting on connection loss, or transmitting only changed filesystem blocks rather than full partition images.

</details>

---

### Scenario-Based

**Q10.** Your company manufactures smart agricultural sensors deployed in remote areas with cellular connectivity. The devices have limited bandwidth (often 2G speeds), small flash storage (4MB total), and operate on solar power with limited battery backup. You need to design an OTA update strategy for a fleet of 25,000 devices spread across 12 countries. Describe your approach addressing: partition strategy, update distribution method, rollback mechanism, and deployment pattern.

<details>
<summary>Answer</summary>

**Partition Strategy**: Given the 4MB flash constraint, a full A/B scheme may not be feasible. Instead, implement A/B partitioning only for critical bootloader and core OS components (approximately 1MB each), with a single application partition and a recovery partition. Alternatively, use a recovery-based scheme where a minimal recovery OS can restore the main partition from a downloaded image stored temporarily in external storage or transmitted in chunks.

**Update Distribution Method**: Delta updates are essential given 2G bandwidth constraints. Implement binary diff algorithms (like bsdiff) to transmit only changes. Add strong compression (LZMA for high compression ratio despite slower decompression). Support resumable downloads with chunk-based transfers and checksums per chunk to handle unreliable connections. Schedule downloads during optimal solar charging periods to ensure sufficient power.

**Rollback Mechanism**: Implement a boot counter in persistent storage (EEPROM or flash wear-leveled area). After three failed boots, automatically revert to the recovery partition. Store a "last known good" configuration separately from the firmware. Include a watchdog timer that triggers rollback if the application doesn't signal healthy operation within 5 minutes of boot.

**Deployment Pattern**: Use ring-based deployment with geographic segmentation—start with devices in regions with better connectivity. Ring 1 (1%): internal test devices with good connectivity. Ring 2 (5%): diverse geographic sample. Ring 3 (20%): broader rollout. Ring 4 (remaining): full fleet. Require 48-72 hour observation periods between rings given the remote nature and potential communication delays. Set auto-rollback triggers at 2% failure rate above baseline. Consider time-zone-aware scheduling to update during local daylight hours when solar power is available.

</details>