## Chapter 4: Giving Devices Their Identity — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | What is an X.509 certificate? | A digital certificate that uses the X.509 PKI standard to bind a public key to an identity, enabling cryptographic verification of device authenticity |
| 2 | What are the three levels in a typical X.509 certificate hierarchy for IoT? | Root CA (trust anchor), Intermediate CA (issues device certs), and End-entity/Device certificates (individual device identity) |
| 3 | What is zero-touch provisioning (ZTP)? | An automated process where devices securely onboard and configure themselves without manual intervention, typically using pre-installed credentials from manufacturing |
| 4 | What is Azure Device Provisioning Service (DPS)? | A helper service for Azure IoT Hub that enables zero-touch, just-in-time provisioning of devices to the correct IoT hub without human intervention |
| 5 | What three attestation mechanisms does Azure DPS support? | TPM attestation, X.509 certificate attestation, and Symmetric key attestation |
| 6 | What is AWS IoT Device Management's fleet provisioning? | A feature that automatically registers and configures devices at scale using provisioning templates and claim certificates |
| 7 | What is a Hardware Security Module (HSM)? | A dedicated physical computing device that safeguards cryptographic keys and performs encryption operations in tamper-resistant hardware |
| 8 | What is a Trusted Platform Module (TPM)? | A specialized chip on a device that securely stores cryptographic keys, certificates, and passwords, and provides hardware-based security functions |
| 9 | Why is hardware-based key storage preferred over software-based storage? | Hardware storage is tamper-resistant, keys never leave the secure element, and it's immune to software-based extraction attacks |
| 10 | What is cryptographic attestation in IoT? | The process of proving a device's identity and integrity through cryptographic evidence, typically using certificates or TPM-based measurements |
| 11 | What is certificate rotation and why is it needed? | The process of replacing expiring certificates with new ones before expiration to maintain continuous secure operations and limit exposure from compromised keys |
| 12 | What challenges exist with certificate rotation at scale? | Coordinating updates across thousands of devices, handling offline devices, ensuring zero downtime, and managing certificate revocation lists |
| 13 | What is the role of certificate revocation in device identity management? | It invalidates compromised or decommissioned device certificates, preventing unauthorized devices from connecting to the IoT infrastructure |
| 14 | What happens if a device's identity cannot be cryptographically verified? | The device should be denied access to the IoT platform, as unverified identity undermines all other security controls |
| 15 | What is enrollment group provisioning? | A method of registering multiple devices simultaneously using a shared intermediate CA certificate or symmetric key, simplifying large-scale deployments |

### Key Terms

| Term | Definition |
|------|-----------|
| X.509 Certificate | A standard format for public key certificates that binds identity information to a cryptographic public key |
| Zero-Touch Provisioning | Automated device onboarding that requires no manual configuration or human intervention |
| TPM (Trusted Platform Module) | A hardware chip providing secure cryptographic key storage and platform integrity verification |
| HSM (Hardware Security Module) | Dedicated hardware for secure key management and cryptographic operations |
| Certificate Authority (CA) | A trusted entity that issues and manages digital certificates within a PKI |
| Attestation | Cryptographic proof that a device is what it claims to be and hasn't been tampered with |
| DPS (Device Provisioning Service) | Azure's service for automated, scalable IoT device provisioning |
| Certificate Rotation | The scheduled replacement of certificates before expiration to maintain security |

### Memory Tricks
- **"Root-Inter-Device" = RID yourself of identity problems**: Remember the X.509 hierarchy flows from Root CA → Intermediate CA → Device certificate
- **"TPM = Trust Permanently in Module"**: The TPM chip is your permanent trust anchor that never exposes keys
- **"ZTP = Zero Touching, Perfect setup"**: Zero-touch provisioning means hands-off, automated device configuration
- **"No ID, No Entry"**: Like a bouncer checking IDs, without verified cryptographic identity, devices get rejected—this is your security foundation