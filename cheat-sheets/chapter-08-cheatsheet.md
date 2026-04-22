## Chapter 8: When Things Go Dark — Quick Reference

### Core Concepts
| Concept | One-line explanation |
|---------|---------------------|
| Device-side buffering | Store messages locally when network unavailable, forward when connectivity returns |
| Exponential backoff | Double wait time between retry attempts to avoid overwhelming recovering servers |
| Jitter | Add randomness to retry timing to prevent synchronized reconnection floods |
| Reconnection storms | Mass simultaneous reconnects after outage that can crash recovering infrastructure |
| Idempotent commands | Operations that produce same result whether executed once or multiple times |
| Message deduplication | Detecting and discarding duplicate messages from retransmissions |

### Key Syntax / Commands
```
# Exponential backoff with jitter formula
wait_time = min(base_delay * (2 ^ attempt) + random(0, jitter_max), max_delay)

# Example: base=1s, attempt=3, jitter_max=1s, max_delay=60s
wait_time = min(1 * 8 + random(0,1), 60) = ~8-9 seconds

# Idempotency key pattern
message_id = device_id + timestamp + sequence_number
```

### Common Patterns
**Pattern 1: Circular Buffer Storage**
Fixed-size local storage overwrites oldest data when full; preserves recent readings during extended outages without memory overflow.

**Pattern 2: Store-and-Forward Queue**
Persist messages to flash/SD, maintain send pointer, acknowledge only after server confirmation, resume from pointer on reconnect.

**Pattern 3: Deduplication Window**
Server maintains sliding window of recently processed message IDs; reject duplicates within TTL period.

### Things to Remember
✅ Always assume network WILL fail—design offline-first
✅ Include unique message IDs at the source for deduplication
✅ Set maximum retry caps to prevent infinite loops
✅ Buffer capacity should match expected outage duration × data rate
❌ Never use fixed retry intervals—causes thundering herd
❌ Don't trust "delivered" status—confirm with application-level ACK

### Quick Quiz
1. Why add jitter to backoff? → Prevents synchronized retries causing secondary outages
2. What makes a command idempotent? → Same outcome regardless of execution count (e.g., "set temp=72" vs "increase temp by 2")