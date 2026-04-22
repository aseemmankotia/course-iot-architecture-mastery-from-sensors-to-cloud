## Chapter 12: Platforms That Scale — Practice Questions

### Multiple Choice

**Q1.** In a multi-tenant IoT platform, which tenancy model provides the strongest isolation but typically has the highest operational cost?

A) Pooled tenancy
B) Bridge tenancy
C) Siloed tenancy
D) Hybrid tenancy

<details>
<summary>Answer</summary>

**Correct: C**

Siloed tenancy provides each tenant with dedicated, isolated infrastructure components, offering the strongest isolation and security guarantees. However, this comes at the highest cost because resources cannot be shared across tenants. Pooled tenancy (A) shares resources and is most cost-effective. Bridge tenancy (B) is a hybrid approach balancing isolation and cost. Hybrid tenancy (D) is not a standard term in this context.

</details>

---

**Q2.** When implementing tenant-scoped partitioning for time-series IoT data, which strategy best supports both tenant isolation and efficient querying within a tenant's data?

A) Single global table with tenant ID column only
B) Composite partition key using tenant ID and time-based bucketing
C) Separate database instance per device
D) Random distribution across partitions

<details>
<summary>Answer</summary>

**Correct: B**

A composite partition key combining tenant ID with time-based bucketing ensures tenant data is logically separated while also distributing the data temporally for efficient range queries. Option A creates hot partitions and poor query performance. Option C is impractical at scale with potentially millions of devices. Option D destroys query locality and makes tenant-specific queries extremely inefficient.

</details>

---

**Q3.** A platform offers extension points that allow tenants to inject custom business logic when device telemetry arrives. Which pattern is most appropriate for maintaining platform stability?

A) Direct code injection into the main processing pipeline
B) Sandboxed serverless functions with resource limits and timeouts
C) Unrestricted access to the platform's internal APIs
D) Shared thread pool for all tenant custom code

<details>

<summary>Answer</summary>

**Correct: B**

Sandboxed serverless functions provide isolation, resource limits, and timeouts that prevent one tenant's code from affecting others or the core platform. Direct code injection (A) poses severe security and stability risks. Unrestricted API access (C) violates multi-tenant security principles. A shared thread pool (D) allows one tenant's poorly performing code to impact all others.

</details>

---

**Q4.** For accurate cost attribution in a multi-tenant IoT platform, which metric combination provides the most comprehensive view of tenant resource consumption?

A) Only message count
B) Message count, storage volume, compute time, and network egress
C) Number of registered devices only
D) Monthly flat fee regardless of usage

<details>
<summary>Answer</summary>

**Correct: B**

Comprehensive cost attribution requires tracking multiple dimensions of resource usage: message count reflects ingestion costs, storage volume captures persistence costs, compute time accounts for processing and analytics, and network egress measures data transfer costs. Single metrics (A, C) miss significant cost drivers. Flat fees (D) don't attribute costs at all and can lead to unfair subsidization between tenants.

</details>

---

**Q5.** Which compliance requirement most directly necessitates data residency controls in a multi-tenant IoT platform?

A) PCI-DSS for payment processing
B) GDPR's data localization provisions for EU citizens
C) SOC 2 Type II availability requirements
D) ISO 27001 risk assessment guidelines

<details>
<summary>Answer</summary>

**Correct: B**

GDPR includes provisions requiring that personal data of EU citizens may need to remain within the EU or approved jurisdictions, directly necessitating data residency controls. PCI-DSS (A) focuses on cardholder data security but doesn't mandate specific geographic locations. SOC 2 (C) addresses availability but not data residency. ISO 27001 (D) is a framework for security management without specific residency mandates.

</details>

---

### True / False

**Q6.** In a pooled tenancy model, all tenants share the same database instance, making it impossible to provide different service level agreements (SLAs) to different tenants. — **True / False**

*False. While pooled tenancy shares infrastructure, platforms can still offer differentiated SLAs through techniques like quality-of-service tiers, priority queuing, reserved capacity allocations, and rate limiting. Logical partitioning and resource quotas allow tenants to receive different performance guarantees even on shared infrastructure.*

---

**Q7.** Audit logging in a multi-tenant platform should capture the tenant context for every administrative action to support compliance investigations and forensic analysis. — **True / False**

*True. Capturing tenant context in audit logs is essential for compliance frameworks like SOC 2, HIPAA, and GDPR. It enables tenant-specific audit reports, helps identify the scope of security incidents, supports access reviews, and provides the forensic trail needed when investigating potential breaches or policy violations.*

---

### Short Answer

**Q8.** Explain the concept of "noisy neighbor" in multi-tenant IoT platforms and describe two mitigation strategies.

<details>
<summary>Answer</summary>

The "noisy neighbor" problem occurs when one tenant's heavy resource usage negatively impacts other tenants sharing the same infrastructure. In IoT platforms, this might manifest as one tenant's burst of device messages overwhelming shared message brokers or databases, causing latency spikes for all tenants.

Mitigation strategies include:
1. **Rate limiting and throttling**: Enforce per-tenant quotas on message ingestion rates, API calls, and compute consumption to prevent any single tenant from monopolizing resources.
2. **Resource isolation through dedicated capacity**: Assign tenants to separate resource pools or partitions, ensuring that load from one tenant cannot directly impact another's allocated resources.

</details>

---

**Q9.** What is the bridge tenancy model, and when would you choose it over purely pooled or siloed approaches?

<details>
<summary>Answer</summary>

Bridge tenancy is a hybrid approach where some components are shared (pooled) across tenants while other components are dedicated (siloed) per tenant. For example, a platform might share the message ingestion layer and API gateway across all tenants while providing dedicated databases and analytics engines per tenant.

You would choose bridge tenancy when:
- Tenants require strong data isolation for compliance but can tolerate shared compute resources
- Cost optimization is important but certain components need isolation for security or performance
- Different tenants have varying isolation requirements, allowing tiered offerings
- The platform needs to balance operational complexity with tenant customization needs

</details>

---

### Scenario-Based

**Q10.** Your company operates a multi-tenant IoT platform serving industrial customers. A new enterprise client in the healthcare sector wants to onboard 50,000 medical devices but has strict requirements: HIPAA compliance, guaranteed 99.99% uptime SLA, data must remain in the US, and they need custom alerting rules triggered by specific vital sign patterns. Your current platform uses a pooled tenancy model with shared message brokers and databases across all tenants.

Describe your architectural approach to accommodate this client while maintaining the existing platform for other tenants. Address tenancy model selection, data partitioning, compliance controls, and extension mechanisms.

<details>
<summary>Answer</summary>

**Tenancy Model**: Implement a bridge tenancy approach for this client. Keep them on the shared message ingestion layer for cost efficiency, but provision dedicated database clusters and compute resources for their data processing and storage. This provides the isolation needed for HIPAA while leveraging shared infrastructure where appropriate.

**Data Partitioning**: Create a dedicated database cluster in a US region with tenant-scoped partitioning using composite keys (tenant ID + device ID + timestamp). Implement encryption at rest and in transit. Configure replication within US regions only to satisfy data residency requirements.

**Compliance Controls**: 
- Enable comprehensive audit logging with immutable storage for all data access and administrative actions
- Implement role-based access control with the principle of least privilege
- Execute a Business Associate Agreement (BAA) and document all technical controls
- Deploy dedicated encryption keys managed through a HIPAA-compliant key management service
- Establish data retention policies aligned with healthcare regulations

**Extension Mechanisms**: Deploy sandboxed serverless functions (e.g., AWS Lambda, Azure Functions) that subscribe to the tenant's data stream. These functions execute the custom alerting rules for vital sign patterns within the tenant's isolated compute environment, with configurable thresholds and notification endpoints. Use a rule engine that allows the client to define conditions without modifying platform code.

**High Availability**: Provision multi-availability-zone deployments for all dedicated components, implement automated failover, and establish separate monitoring with alerting thresholds appropriate for a 99.99% SLA (approximately 52 minutes of downtime per year maximum).

</details>