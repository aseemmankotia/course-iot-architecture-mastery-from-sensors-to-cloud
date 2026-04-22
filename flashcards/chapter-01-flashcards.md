## Chapter 1: The Connected World Blueprint — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | What are the three layers in the basic IoT architecture model? | Perception Layer, Network Layer, and Application Layer |
| 2 | What is the primary function of the Perception Layer in IoT? | To collect data from the physical environment using sensors, actuators, and devices |
| 3 | What does the Network Layer do in IoT architecture? | Transmits and routes data between devices and processing systems; handles connectivity and communication protocols |
| 4 | What is the role of the Application Layer in IoT? | Processes data, provides user interfaces, and delivers services/insights to end users |
| 5 | How does the Five-layer model expand on the Three-layer model? | It adds Transport Layer (between Perception and Network) and Business Layer (above Application) |
| 6 | What are the three components of the Device-Gateway-Cloud reference architecture? | Devices (sensors/actuators), Gateways (aggregation/translation), and Cloud platforms (processing/storage) |
| 7 | What is the primary function of a gateway in IoT architecture? | Aggregates data from multiple devices, performs protocol translation, and provides local preprocessing |
| 8 | What does CAP theorem state? | A distributed system can only guarantee two of three properties: Consistency, Availability, and Partition tolerance |
| 9 | In IoT systems, which CAP property is typically sacrificed and why? | Often Consistency is relaxed (eventual consistency) because Availability and Partition tolerance are critical for distributed sensors |
| 10 | What is a common data flow pattern from device to cloud? | Device → Gateway → Cloud: Collect → Aggregate/Filter → Process/Store/Analyze |
| 11 | Why is separation of concerns important in IoT architecture? | Enables scalability, easier maintenance, independent updates, and appropriate technology choices at each layer |
| 12 | What type of processing might occur at the gateway level? | Data filtering, aggregation, protocol conversion, edge analytics, and local decision-making |
| 13 | What does "Partition tolerance" mean in CAP theorem for IoT? | The system continues operating despite network failures or communication breakdowns between components |
| 14 | In the Five-layer model, what does the Business Layer handle? | Business logic, system management, decision-making processes, and overall IoT application governance |
| 15 | Why do gateways perform protocol translation? | Devices use various protocols (Zigbee, BLE, LoRa); gateways convert these to standard protocols (MQTT, HTTP) for cloud communication |

### Key Terms

| Term | Definition |
|------|-----------|
| Perception Layer | The bottom layer of IoT architecture containing sensors and actuators that interact with the physical world |
| Network Layer | The middle layer responsible for data transmission, routing, and connectivity between IoT components |
| Gateway | An intermediate device that aggregates data from multiple sensors, translates protocols, and bridges devices to the cloud |
| CAP Theorem | Principle stating distributed systems can only guarantee two of: Consistency, Availability, Partition tolerance |
| Edge Processing | Data processing performed at or near the data source (gateway/device) rather than in the cloud |
| Protocol Translation | Converting data from one communication protocol to another for interoperability |
| Reference Architecture | A standardized blueprint that defines the structure and relationships of components in a system |

### Memory Tricks
- **PNA for Three Layers**: "**P**hone **N**eeds **A**pps" — Perception, Network, Application (bottom to top)
- **CAP as a Hat**: You can only wear 2 parts of a 3-part hat at once — pick Consistency, Availability, or Partition tolerance (choose 2)
- **DGC Flow = "Data Gets Crunched"**: Devices collect, Gateways aggregate, Cloud crunches — remember the data journey
- **Gateway = Translator at the Border**: Think of a gateway like an airport translator who speaks both local languages (device protocols) and international language (cloud protocols)