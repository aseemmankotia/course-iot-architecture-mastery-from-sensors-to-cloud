## Chapter 6: Commanding Your Fleet — Quick Reference

### Core Concepts
| Concept | One-line explanation |
|---------|---------------------|
| Device Twin/Shadow | Cloud-side JSON document representing device state, persists even when device is offline |
| Desired Properties | Cloud-set configuration/commands the device *should* achieve |
| Reported Properties | Device-confirmed state of what it *actually* has applied |
| Direct Methods | Synchronous request-response calls requiring device to be online |
| Eventual Consistency | System design accepting temporary state mismatches that resolve over time |
| Offline Convergence | Process where devices sync to desired state upon reconnection |

### Key Syntax / Commands
```json
// Device Twin Structure
{
  "deviceId": "sensor-001",
  "properties": {
    "desired": { "telemetryInterval": 30, "$version": 5 },
    "reported": { "telemetryInterval": 60, "$version": 12 }
  },
  "tags": { "location": "building-A", "floor": 3 }
}

// Direct Method Invocation
{ "methodName": "reboot", "payload": { "delay": 10 }, "timeoutInSeconds": 30 }
```

### Common Patterns
**Pattern 1: Desired/Reported Reconciliation Loop**
Device wakes → reads desired → applies changes → updates reported → cloud confirms convergence

**Pattern 2: Version-Based Conflict Resolution**
Always include `$version` in updates; reject stale writes; last-write-wins or merge strategies

**Pattern 3: Command Fallback Chain**
Try direct method → if offline, set desired property → device processes on reconnection

### Things to Remember
✅ Design for offline-first: assume devices will miss real-time commands
✅ Use desired properties for config; direct methods for immediate actions
✅ Version all state changes to detect and resolve conflicts
✅ Tags are cloud-only metadata; properties sync bidirectionally
❌ Don't rely on synchronous calls for critical operations—devices disconnect

### Quick Quiz
1. When should you use direct methods vs desired properties? → Direct methods for immediate, synchronous actions (reboot); desired properties for persistent config changes
2. What happens to desired properties when a device is offline? → They persist in the twin; device reads and applies them upon reconnection