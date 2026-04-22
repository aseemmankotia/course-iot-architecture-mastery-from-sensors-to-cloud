## Chapter 12: Platforms That Scale — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | What is multi-tenancy in IoT platform architecture? | An architecture where a single platform instance serves multiple customers (tenants) while maintaining logical separation of their data and resources. |
| 2 | What are the three main tenancy models for IoT platforms? | Pooled (shared resources), Siloed (dedicated resources per tenant), and Bridge (hybrid approach combining elements of both). |
| 3 | What is the pooled tenancy model? | All tenants share the same infrastructure, databases, and compute resources, with logical separation through tenant identifiers. Most cost-efficient but lowest isolation. |
| 4 | What is the siloed tenancy model? | Each tenant gets dedicated, isolated infrastructure (separate databases, compute instances). Highest isolation and security but most expensive. |
| 5 | What is the bridge tenancy model? | A hybrid approach where some resources are shared (e.g., API gateways) while critical components (e.g., databases) are isolated per tenant. |
| 6 | What is tenant-scoped partitioning? | A strategy for organizing data where every record is tagged with a tenant identifier, ensuring queries only return data belonging to that specific tenant. |
| 7 | Name three tenant-scoped partitioning strategies. | 1) Discriminator column (tenant_id in shared tables), 2) Schema per tenant (separate schemas, shared database), 3) Database per tenant (complete separation). |
| 8 | What are extension points in multi-tenant platforms? | Defined interfaces where tenants can inject custom logic, configurations, or integrations without modifying core platform code (e.g., webhooks, plugins, custom rules). |
| 9 | What is cost attribution in multi-tenant IoT platforms? | The process of tracking and allocating infrastructure costs (compute, storage, bandwidth) to specific tenants based on their actual resource consumption. |
| 10 | What is metering in the context of IoT platforms? | Measuring tenant usage of platform resources (messages processed, devices connected, API calls) for billing, quotas, and capacity planning. |
| 11 | Why is the "noisy neighbor" problem a concern in pooled tenancy? | One tenant's heavy resource usage can degrade performance for all other tenants sharing the same infrastructure. |
| 12 | What compliance requirement is most challenging in pooled tenancy models? | Data residency requirements—ensuring tenant data stays in specific geographic regions when infrastructure is shared globally. |
| 13 | What makes a data leak "impossible" vs "merely unlikely" in multi-tenant systems? | Physical/infrastructure isolation (siloed) makes leaks impossible; logical isolation (pooled) relies on software correctness, making leaks unlikely but possible due to bugs. |
| 14 | What are common audit requirements for multi-tenant IoT platforms? | Access logging, data lineage tracking, change management records, tenant isolation verification, and breach notification capabilities. |
| 15 | When should you choose siloed over pooled tenancy? | When handling highly sensitive data (healthcare, finance), strict regulatory requirements exist, tenants demand SLA guarantees, or noisy neighbor risks are unacceptable. |

### Key Terms

| Term | Definition |
|------|-----------|
| Pooled Tenancy | Shared infrastructure model where all tenants use common resources with logical separation via tenant identifiers. |
| Siloed Tenancy | Isolated infrastructure model where each tenant receives dedicated, physically separated resources. |
| Bridge Tenancy | Hybrid model balancing shared and dedicated resources based on security/cost requirements. |
| Tenant-scoped Partitioning | Data organization strategy ensuring all queries and operations are filtered by tenant identity. |
| Cost Attribution | Process of tracking and assigning infrastructure expenses to individual tenants based on usage. |
| Metering | Measurement of resource consumption per tenant for billing, throttling, and capacity planning. |
| Extension Points | Customization interfaces allowing tenants to add functionality without altering core platform code. |
| Noisy Neighbor | Problem where one tenant's resource consumption negatively impacts other tenants' performance. |

### Memory Tricks
- **"PSB" = Pool Saves Bucks, Silo Secures Best, Bridge Balances Both** — Remember the three models and their primary tradeoffs.
- **"CALM" for multi-tenant requirements** — Cost attribution, Audit trails, Logical isolation, Metering.
- **Think of an apartment building**: Pooled = shared laundry room (cheap, crowded); Siloed = private washer/dryer in each unit (expensive, guaranteed access); Bridge = shared building amenities but private bathrooms.