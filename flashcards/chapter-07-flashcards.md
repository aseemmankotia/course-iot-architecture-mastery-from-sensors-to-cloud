## Chapter 7: Updates That Don't Brick Your Fleet — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | What is an A/B partition scheme in IoT firmware updates? | A dual-partition system where one partition runs the active firmware while the other receives updates, allowing instant rollback by switching boot partitions if an update fails. |
| 2 | How does bootloader rollback protect IoT devices? | The bootloader tracks boot attempts and automatically reverts to the previous working partition if the new firmware fails to boot successfully after a set number of tries. |
| 3 | What is a canary deployment in IoT fleet management? | A deployment strategy where updates are first pushed to a small subset (1-5%) of devices to detect issues before rolling out to the entire fleet. |
| 4 | How do ring-based deployment patterns work? | Devices are organized into concentric rings (e.g., internal test, beta users, regional groups, full fleet), with updates propagating outward only after success in inner rings. |
| 5 | What are delta updates and why are they used? | Updates containing only the binary differences between firmware versions, reducing bandwidth usage by 70-90% compared to full image downloads. |
| 6 | What is a convergence metric in fleet updates? | A measurement tracking what percentage of devices have successfully updated to the target firmware version within a specified timeframe. |
| 7 | What triggers an auto-rollback during firmware deployment? | Predefined thresholds such as boot failure rates exceeding 2%, crash loops, connectivity loss, or health check failures across the updated device cohort. |
| 8 | Why is firmware signing essential for IoT security? | It ensures authenticity and integrity by cryptographically proving the firmware originated from a trusted source and hasn't been tampered with during transit. |
| 9 | What is the typical verification flow for signed firmware? | Device receives update → extracts signature → verifies signature against stored public key → checks firmware hash → proceeds with installation only if all checks pass. |
| 10 | What is a "brick" in IoT context and why is prevention critical? | A device rendered permanently non-functional by a failed update; prevention is critical because physical access for recovery is often impossible or prohibitively expensive at scale. |
| 11 | What percentage of devices should ideally be affected by a bad firmware update with proper architecture? | Around 1% or less, caught during canary deployment with automatic rollback, rather than 100% requiring manual intervention. |
| 12 | How does bsdiff/bspatch enable delta updates? | These algorithms compute binary differences between old and new firmware images, creating compact patch files that can reconstruct the new version from the old. |
| 13 | What is a watchdog timer's role in firmware update safety? | It monitors device health post-update and triggers automatic reboot/rollback if the device becomes unresponsive or fails to "kick" the watchdog within expected intervals. |
| 14 | What are typical ring deployment stages for enterprise IoT? | Ring 0: Internal/test devices → Ring 1: Canary (1%) → Ring 2: Early adopters (10%) → Ring 3: Broad deployment (50%) → Ring 4: Full fleet (100%). |
| 15 | What metadata should convergence dashboards track? | Update success/failure rates, rollback counts, time-to-convergence, devices pending update, version distribution across fleet, and error categorization. |

### Key Terms

| Term | Definition |
|------|-----------|
| A/B Partitioning | Dual-partition firmware scheme enabling atomic updates and instant rollback by maintaining two complete system images. |
| Canary Deployment | Release strategy testing updates on a small device subset before broader rollout to catch issues early. |
| Delta Update | Bandwidth-efficient update containing only differences between firmware versions rather than complete images. |
| Convergence Metric | Measurement of fleet-wide update adoption progress, typically expressed as percentage of devices on target version. |
| Firmware Signing | Cryptographic process using private keys to create verifiable signatures proving firmware authenticity and integrity. |
| Auto-Rollback | Automated reversion to previous firmware version triggered when health metrics exceed failure thresholds. |
| Watchdog Timer | Hardware/software timer that resets devices if not periodically refreshed, detecting hung or crashed systems. |

### Memory Tricks
- **A/B = Always Backup**: Remember A/B partitions as "Always having a Backup" ready to boot.
- **Canary in the Coal Mine**: Just like miners' canaries detected danger first, canary devices detect bad updates before they harm the whole fleet.
- **DELTA = Difference Enables Lightweight Transfer Always**: Delta updates send only what's Different.
- **Ring Deployment = Ripples in a Pond**: Updates spread outward like ripples—start small in the center, expand gradually to the edges.