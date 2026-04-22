## Chapter 4: Giving Devices Their Identity — Practice Questions

### Multiple Choice

**Q1.** In an X.509 certificate hierarchy, which entity is responsible for signing device certificates and serves as an intermediate trust anchor between the root CA and end devices?

A) The root Certificate Authority
B) An Intermediate Certificate Authority
C) The device's Trusted Platform Module
D) The cloud provisioning service

<details>
<summary>Answer</summary>

**Correct: B**

An Intermediate Certificate Authority (ICA) sits between the root CA and end-entity certificates, signing device certificates while being signed by the root CA. The root CA (A) typically signs intermediate CAs, not device certificates directly, to limit exposure. The TPM (C) stores keys but doesn't sign certificates in the hierarchy. Cloud provisioning services (D) verify certificates but don't issue them in the certificate chain.

</details>

---

**Q2.** During zero-touch provisioning, what is the primary purpose of the device sending an attestation to the provisioning service?

A) To download firmware updates before registration
B) To prove the device's identity without manual configuration
C) To establish a direct connection to the application database
D) To negotiate the billing terms for cloud services

<details>
<summary>Answer</summary>

**Correct: B**

Attestation in zero-touch provisioning allows devices to cryptographically prove their identity to the provisioning service without requiring manual intervention or pre-configuration. Firmware updates (A) may happen after provisioning but aren't the attestation's purpose. Application database connections (C) occur after provisioning completes. Billing negotiations (D) are administrative functions unrelated to device attestation.

</details>

---

**Q3.** When using AWS IoT Device Management, which feature enables you to organize devices into logical groups for bulk operations like certificate rotation?

A) Thing Types
B) Thing Groups
C) Fleet Indexing
D) Device Shadows

<details>
<summary>Answer</summary>

**Correct: B**

Thing Groups in AWS IoT Device Management allow you to organize devices into hierarchical groups for applying policies and performing bulk operations like certificate rotation. Thing Types (A) define common attributes but don't enable bulk operations. Fleet Indexing (C) enables searching and querying devices but isn't used for grouping operations. Device Shadows (D) maintain device state but don't organize devices for bulk management.

</details>

---

**Q4.** What is the primary security advantage of using a Hardware Security Module (HSM) or TPM for storing device private keys?

A) It increases network bandwidth for certificate exchanges
B) Private keys never leave the secure hardware boundary
C) It eliminates the need for certificate expiration dates
D) It automatically updates device firmware

<details>
<summary>Answer</summary>

**Correct: B**

HSMs and TPMs provide tamper-resistant storage where private keys are generated and used within the secure boundary, never exposed in extractable form. This prevents key theft even if the device software is compromised. Network bandwidth (A) is unrelated to secure key storage. Certificates still require expiration dates (C) regardless of where keys are stored. Firmware updates (D) are a separate function from cryptographic key protection.

</details>

---

**Q5.** In Azure Device Provisioning Service (DPS), what mechanism allows a device to be automatically assigned to the appropriate IoT Hub based on custom logic?

A) Symmetric key attestation
B) Enrollment groups
C) Custom allocation policies
D) Individual enrollment

<details>
<summary>Answer</summary>

**Correct: C**

Custom allocation policies in Azure DPS allow you to implement Azure Functions that contain custom logic to determine which IoT Hub a device should be assigned to during provisioning. Symmetric key attestation (A) is an authentication method, not an allocation mechanism. Enrollment groups (B) define how devices are enrolled but use allocation policies for hub assignment. Individual enrollment (D) pre-registers specific devices but doesn't provide dynamic allocation logic.

</details>

---

### True / False

**Q6.** A Trusted Platform Module (TPM) can generate cryptographic keys internally, ensuring the private key is never exposed outside the chip. — **True / False**

<details>
<summary>Answer</summary>

**True**

TPMs are designed to generate key pairs within their secure boundary. The private key remains protected inside the TPM and cryptographic operations using it are performed internally. Only the public key is exported for sharing. This architectural design is fundamental to TPM security and prevents key extraction even through software attacks on the host device.

</details>

---

**Q7.** Certificate rotation at scale requires all devices to be offline simultaneously to ensure consistent security state across the fleet. — **True / False**

<details>
<summary>Answer</summary>

**False**

Certificate rotation at scale is specifically designed to happen without requiring device downtime. Best practices involve issuing new certificates before old ones expire, supporting dual-certificate operation during transition periods, and using gradual rollout strategies. Taking all devices offline would defeat the purpose of maintaining continuous operations and would be impractical for large IoT deployments.

</details>

---

### Short Answer

**Q8.** Explain the difference between individual enrollment and group enrollment in the context of IoT device provisioning services.

<details>
<summary>Answer</summary>

Individual enrollment creates a provisioning entry for a single, specific device using its unique identifier (such as a device certificate or TPM endorsement key). This approach is used when a device needs custom initial configuration or when provisioning high-value devices that require individual tracking.

Group enrollment allows multiple devices to be provisioned using a shared trust anchor, typically an intermediate CA certificate. Any device presenting a certificate signed by that CA can be automatically provisioned. This approach scales better for manufacturing and is used when many devices share common initial configurations.

</details>

---

**Q9.** Describe two scenarios where certificate rotation at scale becomes necessary and how you would approach planning for it.

<details>
<summary>Answer</summary>

**Scenario 1: Certificate Expiration** — Certificates have validity periods and must be replaced before expiration. Planning involves tracking expiration dates across the fleet, issuing replacement certificates with sufficient lead time (weeks or months), and implementing gradual rollout to detect issues before affecting all devices.

**Scenario 2: Security Compromise** — If a signing CA or device keys are compromised, affected certificates must be revoked and replaced. Planning requires maintaining certificate revocation lists (CRLs) or OCSP responders, having emergency rotation procedures documented, and ensuring devices can receive new certificates even when current ones are invalidated.

Additional considerations include testing rotation on small device groups first, maintaining rollback capabilities, and ensuring the provisioning infrastructure can handle the load of mass certificate issuance.

</details>

---

### Scenario-Based

**Q10.** Your company manufactures 50,000 industrial sensors annually. Currently, technicians manually configure each device with certificates during production, taking approximately 8 minutes per device. Leadership wants to reduce this to under 30 seconds while maintaining strong security. The devices have TPM 2.0 chips but no current HSM integration. You're evaluating both Azure DPS and AWS IoT Device Management. 

Describe your recommended provisioning architecture, including: (a) how you would leverage the TPM, (b) your choice of attestation method, and (c) the enrollment strategy that best fits this manufacturing scale.

<details>
<summary>Answer</summary>

**Recommended Architecture:**

**(a) TPM Leverage:** Configure the manufacturing line to initialize each device's TPM during production, generating an Endorsement Key (EK) within the TPM. The TPM should create and store the device's private key internally, with only the public portion exported for registration. This ensures private keys never exist outside the secure hardware.

**(b) Attestation Method:** Use TPM attestation (available in both Azure DPS and AWS IoT). The device proves possession of the TPM-protected private key during provisioning without exposing it. For Azure DPS, this would be TPM attestation with the EK. For AWS, you would use the JITP (Just-In-Time Provisioning) pattern with TPM-based device certificates.

**(c) Enrollment Strategy:** Implement group enrollment using an intermediate CA certificate that signs device certificates during manufacturing. The manufacturing system would:
1. Initialize TPM and generate device key pair (automated, ~5 seconds)
2. Create CSR using TPM-protected private key
3. Sign certificate with manufacturing CA (automated, ~2 seconds)
4. Register public key/certificate hash with provisioning service in batch uploads
5. Flash minimal bootstrap configuration to device

This reduces per-device time to under 15 seconds of direct interaction, with batch certificate registration happening asynchronously. Either Azure DPS or AWS IoT Device Management can support this pattern, with the choice depending on your existing cloud infrastructure and multi-region requirements.

</details>

---