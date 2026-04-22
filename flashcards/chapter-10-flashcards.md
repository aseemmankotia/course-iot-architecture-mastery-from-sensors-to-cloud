## Chapter 10: Locking Down the Connected World — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | What is Zero Trust architecture in the context of IoT? | A security model that assumes no device, user, or network is inherently trusted—every access request must be verified regardless of location or previous authentication. |
| 2 | What is the core principle of "assume breach" in IoT security? | Design systems expecting that compromise will occur, focusing on limiting damage through blast radius reduction rather than solely preventing initial intrusion. |
| 3 | What is network segmentation in IoT environments? | Dividing the network into isolated zones so IoT devices are separated from critical systems, limiting attacker movement if one segment is compromised. |
| 4 | What is lateral movement and why is it dangerous in IoT? | When attackers move from one compromised device to access others on the network; dangerous because IoT devices often share networks with sensitive systems. |
| 5 | What are the three pillars of blast radius reduction? | Segmentation, least privilege, and continuous verification. |
| 6 | What is secrets management in IoT systems? | The practice of securely storing, distributing, and accessing sensitive credentials like API keys, certificates, and passwords used by devices. |
| 7 | Why is key rotation important for IoT security? | Limits the window of exposure if a key is compromised and reduces the value of stolen credentials over time. |
| 8 | What is data-plane encryption? | Encrypting the actual payload data transmitted between IoT devices and backend systems, protecting information in transit. |
| 9 | What is the difference between control-plane and data-plane encryption? | Control-plane encrypts management/signaling traffic; data-plane encrypts the actual sensor data and operational messages. |
| 10 | What is threat modeling for connected devices? | Systematically identifying potential threats, vulnerabilities, and attack vectors specific to IoT deployments to prioritize security controls. |
| 11 | What does "least privilege" mean for IoT devices? | Granting devices only the minimum permissions and network access required to perform their specific function—nothing more. |
| 12 | What is continuous verification in Zero Trust? | Ongoing authentication and authorization checks throughout a session, not just at initial connection, detecting anomalous behavior. |
| 13 | Why should IoT security be architectural rather than bolted-on? | Retrofitting security is expensive, incomplete, and leaves gaps; security designed into the architecture provides comprehensive, cost-effective protection. |
| 14 | What is micro-segmentation in IoT networks? | Granular network isolation that creates secure zones around individual devices or small groups, limiting communication to only what's explicitly allowed. |
| 15 | What role do hardware security modules (HSMs) play in IoT secrets management? | They provide tamper-resistant storage for cryptographic keys, enabling secure key generation, storage, and operations on devices. |

### Key Terms

| Term | Definition |
|------|-----------|
| Zero Trust | Security framework requiring strict identity verification for every person and device accessing resources, regardless of network location. |
| Blast Radius | The scope of damage an attacker can cause after compromising a system; smaller is better. |
| Lateral Movement | Technique attackers use to move through a network after initial compromise, seeking valuable targets. |
| Network Segmentation | Splitting a network into isolated subnetworks to contain breaches and control traffic flow. |
| Key Rotation | Regularly replacing cryptographic keys to limit exposure from potential compromise. |
| Data-Plane Encryption | Protecting actual transmitted data (versus management traffic) using cryptographic methods like TLS or DTLS. |
| Threat Modeling | Structured approach to identifying security threats and vulnerabilities during system design. |
| Secrets Management | Tools and practices for securely handling credentials, keys, and other sensitive configuration data. |

### Memory Tricks
- **"Never Trust, Always Verify"** — The Zero Trust mantra; imagine every device wearing a disguise that must be removed each time.
- **"SLC for Blast Control"** — Segmentation, Least privilege, Continuous verification = the three controls that shrink your blast radius.
- **"Rotate Before It's Too Late"** — Picture keys rusting over time; rotation keeps them fresh and attackers frustrated.