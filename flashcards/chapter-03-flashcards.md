## Chapter 3: Where Intelligence Lives — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | What are the three main computing paradigms in IoT architecture? | Edge, Fog, and Cloud computing - representing a spectrum from device-local to centralized processing |
| 2 | Where does Edge computing process data? | Directly at or near the data source (sensors/devices), typically within milliseconds of data generation |
| 3 | What is Fog computing and where does it sit in the architecture? | An intermediate layer between Edge and Cloud that aggregates data from multiple edge nodes, providing regional processing capabilities |
| 4 | What latency can Edge computing achieve compared to Cloud? | Sub-millisecond responses at the edge vs. 50-200+ milliseconds for cloud round-trips |
| 5 | By how much can Edge computing reduce bandwidth costs? | Up to 80% by filtering and processing data locally before transmission |
| 6 | What is an Edge gateway's primary function? | To bridge IoT devices with higher-level networks while performing local data processing, filtering, and protocol translation |
| 7 | What is data filtering at the edge? | Removing redundant, irrelevant, or low-value data locally so only meaningful information is sent upstream |
| 8 | Name three latency reduction strategies in IoT architecture | 1) Process at edge, 2) Cache frequently accessed data locally, 3) Use predictive prefetching of cloud resources |
| 9 | What is an offline resilience pattern? | A design approach that allows edge systems to continue functioning when cloud connectivity is lost, then sync when restored |
| 10 | Why is hybrid edge-cloud architecture becoming the production standard? | It balances real-time local processing needs with cloud scalability, analytics, and long-term storage |
| 11 | What type of processing should remain at the edge vs. cloud? | Edge: time-critical decisions, initial filtering. Cloud: complex analytics, ML training, historical analysis |
| 12 | What is store-and-forward in offline resilience? | Buffering data locally during connectivity loss and transmitting it when the connection is restored |
| 13 | How does an edge gateway handle protocol translation? | It converts between device protocols (Zigbee, BLE, Modbus) and standard IP/cloud protocols (MQTT, HTTPS) |
| 14 | What is local inference in edge computing? | Running pre-trained ML models on edge devices to make predictions without cloud connectivity |
| 15 | When would you choose Cloud over Edge processing? | For computationally intensive tasks, cross-device correlation, model training, compliance logging, or long-term trend analysis |

### Key Terms

| Term | Definition |
|------|-----------|
| Edge Computing | Processing data at or near the source device, minimizing latency and bandwidth usage |
| Fog Computing | A distributed layer between edge and cloud providing regional aggregation and intermediate processing |
| Edge Gateway | A device that connects IoT sensors to networks, handling protocol translation, security, and local processing |
| Data Filtering | Selectively discarding or summarizing raw data at the edge to reduce transmission volume |
| Offline Resilience | System's ability to maintain core functionality during network disconnection |
| Store-and-Forward | Buffering mechanism that queues data locally during outages for later transmission |
| Local Inference | Executing ML model predictions directly on edge devices without cloud dependency |

### Memory Tricks
- **"EFC = Elevator Floor Choices"**: Edge (ground floor/closest), Fog (middle floors), Cloud (top floor/furthest) - the higher you go, the more you can see but the longer the elevator ride
- **"80-20 Edge Rule"**: Edge filters 80% of data, sends 20% to cloud - like a coffee filter keeping the grounds (noise) while letting the good stuff through
- **"GOLF for Offline Resilience"**: **G**raceful degradation, **O**ffline queuing, **L**ocal decisions, **F**orward when connected