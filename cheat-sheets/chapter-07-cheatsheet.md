## Chapter 7: Updates That Don't Brick Your Fleet — Quick Reference

### Core Concepts
| Concept | One-line explanation |
|---------|---------------------|
| A/B Partition Scheme | Dual partition layout allowing updates to inactive slot while current runs, enabling instant rollback |
| Bootloader Rollback | Boot mechanism that detects failed updates and automatically reverts to last-known-good partition |
| Canary Deployment | Release updates to small subset (1-5%) first, monitor health before wider rollout |
| Ring-based Deployment | Staged rollout through concentric groups: internal → beta → early adopters → general |
| Delta Updates | Send only binary differences between versions, reducing bandwidth by 60-90% |
| Convergence Metrics | Measurements tracking fleet update progress and health (success rate, install time, error codes) |
| Firmware Signing | Cryptographic signature ensuring update authenticity and integrity before installation |

### Key Syntax / Commands
```
# Typical A/B partition layout
/dev/mmcblk0p1  boot_a      /dev/mmcblk0p2  boot_b
/dev/mmcblk0p3  system_a    /dev/mmcblk0p4  system_b
/dev/mmcblk0p5  data (persistent)

# Signature verification (conceptual)
openssl dgst -sha256 -verify pubkey.pem -signature fw.sig firmware.bin

# Rollback trigger pseudocode
if (boot_count > 3 && !health_check_passed): switch_partition()
```

### Common Patterns
**Pattern 1: Canary with Auto-Rollback**
Deploy to 1% → monitor 24hrs → check error rate < 0.1% → expand to 10% → repeat until 100%

**Pattern 2: Health Watchdog**
Device reports heartbeat post-update; 3 consecutive failures trigger automatic partition swap

### Things to Remember
✅ Always maintain a golden recovery partition that NEVER gets updated
✅ Set convergence thresholds BEFORE deployment (e.g., >95% success in 48hrs)
✅ Sign firmware at build time, verify at device before AND after download
❌ Never update bootloader and application in same operation—brick risk doubles

### Quick Quiz
1. Why A/B over single partition? → Enables atomic updates with instant rollback; no half-written state
2. When should auto-rollback trigger? → When convergence metrics breach threshold (error rate, boot loops, missed heartbeats)