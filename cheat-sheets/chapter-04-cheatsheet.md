## Chapter 4: Giving Devices Their Identity — Quick Reference

### Core Concepts
| Concept | One-line explanation |
|---------|---------------------|
| X.509 Certificate Hierarchies | Chain of trust from Root CA → Intermediate CA → Device certificates for identity verification |
| Zero-touch Provisioning | Automated device onboarding without manual intervention using pre-loaded credentials |
| Azure DPS | Device Provisioning Service that auto-assigns devices to IoT hubs using attestation |
| AWS IoT Device Management | Fleet provisioning with Just-in-Time Registration (JITR) and bulk registration |
| Hardware Security Module (HSM) | Dedicated crypto processor for secure key storage and operations |
| TPM (Trusted Platform Module) | Hardware chip providing secure key generation, storage, and attestation |
| Certificate Rotation | Scheduled replacement of certificates before expiry to maintain security |

### Key Syntax / Commands
```bash
# Generate device certificate signed by CA
openssl req -new -key device.key -out device.csr
openssl x509 -req -in device.csr -CA intermediate.crt -CAkey intermediate.key -out device.crt

# AWS IoT - Register CA certificate
aws iot register-ca-certificate --ca-certificate file://rootCA.pem --verification-cert file://verificationCert.pem

# Azure DPS - Create enrollment group
az iot dps enrollment-group create --dps-name MyDPS --enrollment-id MyGroup --certificate-path ./root.pem
```

### Common Patterns
**Pattern 1: Manufacturing-Time Identity Injection**
Embed unique certificate + private key into HSM/TPM during factory production; device authenticates on first boot without user action.

**Pattern 2: Claim-Based Provisioning**
Device ships with bootstrap credential, exchanges it for permanent identity after cloud verification of ownership claim.

### Things to Remember
✅ Store private keys in hardware (HSM/TPM)—never in firmware or flash
✅ Use intermediate CAs for device signing; keep Root CA offline
✅ Plan certificate rotation before deployment—retrofitting is painful
❌ Never use shared credentials across devices—compromising one compromises all

### Quick Quiz
1. Why use intermediate CAs instead of signing directly with Root CA? → Root CA stays offline/secure; intermediate can be revoked without rebuilding entire PKI
2. What attestation methods does Azure DPS support? → TPM, X.509 certificates, and Symmetric Key attestation