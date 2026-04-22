## Chapter 10: Locking Down the Connected World — Practice Questions

### Multiple Choice

**Q1.** In a Zero Trust architecture for IoT, what is the fundamental principle that differentiates it from traditional perimeter-based security?

A) All devices inside the network are automatically trusted after initial authentication
B) Every access request must be verified regardless of network location or previous authentication
C) Only devices with static IP addresses are granted network access
D) Trust is established once at device provisioning and maintained throughout the device lifecycle

<details>
<summary>Answer</summary>

**Correct: B**

Zero Trust operates on the principle of "never trust, always verify." Unlike traditional models that trust anything inside the network perimeter, Zero Trust requires continuous verification of every access request. Option A describes the opposite of Zero Trust. Option C is irrelevant to trust architecture. Option D describes a static trust model, which violates Zero Trust principles.

</details>

---

**Q2.** An organization implements network segmentation for their IoT deployment. Which technique is MOST effective at preventing lateral movement between compromised devices?

A) Using a single VLAN for all IoT devices with MAC address filtering
B) Implementing micro-segmentation with device-specific firewall rules and east-west traffic inspection
C) Deploying all IoT devices on a guest network isolated from corporate systems
D) Installing antivirus software on each IoT device

<details>
<summary>Answer</summary>

**Correct: B**

Micro-segmentation creates granular security zones around individual workloads or devices, with specific rules governing traffic between segments. This approach inspects east-west (lateral) traffic, making it most effective against lateral movement. Option A groups all devices together, enabling lateral movement. Option C only isolates from corporate systems, not between IoT devices. Option D is often impractical for resource-constrained IoT devices and doesn't address network-level movement.

</details>

---

**Q3.** When implementing secrets management for IoT devices, what is the recommended approach for storing cryptographic keys on resource-constrained devices?

A) Store keys in plain text configuration files for easy updates
B) Use hardware security modules (HSM) or secure elements when available
C) Embed keys directly in application source code during compilation
D) Store all keys centrally on the cloud server and transmit them on each connection

<details>
<summary>Answer</summary>

**Correct: B**

Hardware security modules and secure elements provide tamper-resistant storage for cryptographic keys, protecting them even if the device software is compromised. Option A exposes keys to anyone with file system access. Option C makes keys extractable through reverse engineering and prevents rotation. Option D exposes keys during transmission and creates a single point of failure.

</details>

---

**Q4.** During threat modeling for a smart building system, which framework provides a structured approach to identifying threats by categorizing them as Spoofing, Tampering, Repudiation, Information disclosure, Denial of service, and Elevation of privilege?

A) OWASP Top 10
B) STRIDE
C) MITRE ATT&CK
D) NIST Cybersecurity Framework

<details>
<summary>Answer</summary>

**Correct: B**

STRIDE is a threat modeling methodology developed by Microsoft that categorizes threats using the six categories listed in the question. OWASP Top 10 focuses on web application vulnerabilities. MITRE ATT&CK is a knowledge base of adversary tactics and techniques. NIST Cybersecurity Framework provides organizational security guidance but is not a threat classification system.

</details>

---

**Q5.** Which data-plane encryption pattern is MOST appropriate for an IoT sensor sending telemetry through an edge gateway to a cloud platform, when the gateway should not access the sensor data content?

A) Transport Layer Security (TLS) between each hop
B) End-to-end encryption from sensor to cloud with payload encryption
C) VPN tunnel from edge gateway to cloud only
D) Application-layer encryption at the gateway before cloud transmission

<details>
<summary>Answer</summary>

**Correct: B**

End-to-end encryption with payload encryption ensures that data is encrypted at the source (sensor) and only decrypted at the final destination (cloud), preventing intermediate nodes like gateways from accessing the content. Option A allows the gateway to decrypt and re-encrypt, exposing data. Option C only protects the gateway-to-cloud segment. Option D requires the gateway to access plaintext data for encryption.

</details>

---

### True / False

**Q6.** In Zero Trust architecture, network location (being inside the corporate firewall) is still considered a factor when granting access to IoT management interfaces. — **True / False**

<details>
<summary>Answer</summary>

**False**

Zero Trust explicitly rejects network location as a trust factor. The principle states that requests should be verified based on multiple signals including user identity, device health, data sensitivity, and behavioral analytics—but never based on whether the request originates from inside or outside the network perimeter. Every request is treated as if it originates from an untrusted network.

</details>

---

**Q7.** Key rotation in IoT deployments should be performed automatically on a regular schedule, even if there is no evidence of key compromise. — **True / False**

<details>
<summary>Answer</summary>

**True**

Proactive key rotation is a security best practice that limits the window of exposure if a key is compromised without detection. Regular rotation reduces the value of stolen keys, limits the amount of data encrypted under any single key, and ensures rotation processes are tested and functional before an emergency requires them. Waiting for evidence of compromise before rotating keys leaves systems vulnerable to undetected breaches.

</details>

---

### Short Answer

**Q8.** Explain why "defense in depth" is particularly important for IoT security compared to traditional IT systems.

<details>
<summary>Answer</summary>

IoT devices often have limited computational resources, making it difficult to implement comprehensive security controls on each device. They frequently operate in physically accessible or uncontrolled environments, increasing exposure to tampering. Many IoT devices have long operational lifespans but receive infrequent security updates. Additionally, the massive scale of IoT deployments increases the attack surface. Defense in depth compensates for these weaknesses by ensuring that if one security layer fails (such as device-level security), other layers (network segmentation, encryption, monitoring) continue to protect the system. No single control can be relied upon, so overlapping protections are essential.

</details>

---

**Q9.** Describe three essential components of an effective secrets management system for an IoT deployment at scale.

<details>
<summary>Answer</summary>

Three essential components include:

1. **Centralized secrets vault**: A secure, dedicated system (like HashiCorp Vault or AWS Secrets Manager) that stores credentials, API keys, and certificates with encryption at rest, access controls, and audit logging.

2. **Automated rotation mechanisms**: Systems that can automatically generate new secrets, distribute them to devices securely, and revoke old secrets without manual intervention or service disruption.

3. **Just-in-time secret delivery**: Rather than storing long-lived secrets on devices, the system provides short-lived credentials or tokens only when needed, reducing the window of exposure if a device is compromised.

Other valid components include: secure bootstrap/provisioning processes, certificate lifecycle management, and integration with device identity systems.

</details>

---

### Scenario-Based

**Q10.** Your company manufactures smart industrial sensors deployed in factories worldwide. A security audit reveals that all 50,000 deployed sensors use the same hardcoded symmetric encryption key for communicating with your cloud platform. The key was embedded during manufacturing. You've been asked to remediate this vulnerability while minimizing operational disruption.

Describe a phased remediation plan that addresses: (a) immediate risk reduction, (b) transitioning to unique per-device keys, and (c) implementing ongoing key management practices.

<details>
<summary>Answer</summary>

**Phase A - Immediate Risk Reduction (1-2 weeks):**
- Implement additional network-layer security (TLS mutual authentication) to add a second encryption layer
- Deploy network segmentation to limit exposure if the shared key is compromised
- Enable enhanced monitoring and anomaly detection for unusual communication patterns
- Assess whether the compromised key has been extracted or exposed publicly

**Phase B - Transition to Unique Per-Device Keys (2-6 months):**
- Design a secure key provisioning protocol that can authenticate devices using the existing shared key while issuing unique device keys
- Implement a device identity system where each sensor receives a unique certificate or key pair
- Roll out firmware updates that support the new key architecture alongside the legacy system
- Gradually migrate devices to unique keys using over-the-air updates, with rollback capability
- Maintain backward compatibility during transition by supporting both authentication methods
- After sufficient migration, revoke the shared key and require unique device authentication

**Phase C - Ongoing Key Management (Continuous):**
- Deploy automated certificate/key rotation with configurable intervals (e.g., 90 days)
- Implement a secrets management platform with audit logging and access controls
- Establish secure key escrow and recovery procedures for device replacement scenarios
- Create monitoring dashboards for key expiration, rotation failures, and anomalies
- Document incident response procedures for key compromise scenarios
- For future manufacturing, implement unique key injection during secure provisioning at factory

</details>