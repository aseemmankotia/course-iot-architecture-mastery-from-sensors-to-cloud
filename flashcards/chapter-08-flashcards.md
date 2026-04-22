## Chapter 8: When Things Go Dark — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | What is device-side buffering in IoT systems? | A strategy where devices store data locally when network connectivity is unavailable, then transmit when connection is restored. |
| 2 | Why is device-side buffering essential for IoT architecture? | It prevents data loss during network outages and ensures eventual data delivery, maintaining system integrity. |
| 3 | What is exponential backoff? | A retry strategy where the wait time between connection attempts doubles after each failure (e.g., 1s, 2s, 4s, 8s). |
| 4 | What is "jitter" in the context of exponential backoff? | A random time variation added to backoff intervals to prevent multiple devices from retrying simultaneously. |
| 5 | What is a reconnection storm? | When thousands of devices attempt to reconnect simultaneously after an outage, overwhelming the server infrastructure. |
| 6 | How does exponential backoff with jitter mitigate reconnection storms? | It spreads reconnection attempts over time randomly, preventing synchronized mass connection requests. |
| 7 | What makes a command "idempotent"? | A command that produces the same result whether executed once or multiple times (e.g., "set temperature to 72°F"). |
| 8 | Why is idempotent command handling critical in IoT systems? | Because network issues may cause commands to be delivered multiple times, and idempotency prevents unintended repeated actions. |
| 9 | What is message deduplication? | The process of identifying and discarding duplicate messages to ensure each command is processed only once. |
| 10 | What are common message deduplication patterns? | Using unique message IDs, timestamps with sequence numbers, or content hashing to identify duplicates. |
| 11 | What is the "store-and-forward" buffering strategy? | Devices queue messages locally and forward them in order once connectivity is restored, preserving message sequence. |
| 12 | How should buffer overflow be handled on resource-constrained devices? | Implement policies like dropping oldest data, prioritizing critical messages, or compressing stored data. |
| 13 | What is the key takeaway for designing IoT systems regarding connectivity? | Design every component assuming the network will fail—this ensures the system gracefully handles real-world conditions. |
| 14 | What information should be stored with buffered messages for proper deduplication? | Message ID, timestamp, sequence number, and optionally a hash of the payload. |
| 15 | Why should retry intervals include a maximum cap in exponential backoff? | To prevent wait times from growing infinitely long, ensuring devices eventually attempt reconnection within reasonable timeframes. |

### Key Terms

| Term | Definition |
|------|-----------|
| Device-side buffering | Local storage of data on IoT devices during network unavailability for later transmission |
| Exponential backoff | A retry algorithm that progressively increases wait time between attempts, typically doubling each interval |
| Jitter | Random variation added to timing intervals to desynchronize multiple devices' actions |
| Reconnection storm | Mass simultaneous reconnection attempts by many devices that can overwhelm server infrastructure |
| Idempotent operation | An operation that yields the same result regardless of how many times it's executed |
| Message deduplication | The process of detecting and eliminating duplicate messages in a distributed system |
| Store-and-forward | A buffering technique where messages are queued locally and transmitted when connectivity returns |

### Memory Tricks
- **JIBE** for retry strategy: **J**itter, **I**ncreasing delays, **B**ackoff, **E**xponential growth
- Think of idempotency like a light switch "SET to ON" vs "TOGGLE"—setting to ON is always safe to repeat, toggling is not
- **"When in doubt, buffer it out"** — always assume the network will fail and plan for local storage
- Reconnection storms are like Black Friday shoppers—add jitter to make them arrive at different times instead of all at once