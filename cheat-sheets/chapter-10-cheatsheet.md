## Chapter 10: Locking Down the Connected World — Quick Reference

### Core Concepts
| Concept | One-line explanation |
|---------|---------------------|
| Zero Trust Architecture | Never trust, always verify—authenticate every request regardless of network location |
| Network Segmentation | Divide networks into isolated zones to contain breaches and block lateral movement |
| Secrets Management | Centralized, secure storage and distribution of credentials, API keys, and certificates |
| Key Rotation | Regularly replacing cryptographic keys to limit exposure window if compromised |
| Data-plane Encryption | Encrypting actual device data in transit (TLS/DTLS) and at rest (AES-256) |
| Threat Modeling | Systematic identification of attack vectors, assets, and mitigations before deployment |

### Key Syntax / Commands
```
# Network segmentation with VLANs (example)
interface vlan 100
  name IoT_Sensors
  ip address 10.10.100.1/24
  access-group IoT_RESTRICTED in

# Vault secrets retrieval
vault kv get -field=api_key secret/iot/device-001

# Certificate rotation check
openssl x509 -enddate -noout -in device_cert.pem
```

### Common Patterns
**Pattern 1: Defense in Depth**
Layer multiple controls—network, device, application, data—so failure of one doesn't mean total compromise.

**Pattern 2: Microsegmentation**
Isolate each device class (sensors, gateways, actuators) into separate network zones with firewall rules between them.

**Pattern 3: Mutual TLS (mTLS)**
Both device and server present certificates, ensuring bidirectional authentication for all communications.

### Things to Remember
✅ Design for breach containment—assume devices WILL be compromised
✅ Automate key rotation; manual processes don't scale for IoT
✅ Apply least privilege: devices get only permissions they absolutely need
✅ Use hardware security modules (HSM/TPM) for key storage on devices
❌ Don't hardcode secrets in firmware—they WILL be extracted
❌ Don't trust devices just because they're "inside" the network

### Quick Quiz
1. What is the core principle of Zero Trust? → Never implicitly trust; verify every request continuously
2. Why segment IoT networks? → Limit blast radius and prevent lateral movement after breach
3. How often should keys rotate? → Based on risk; typically 30-90 days for high-value systems