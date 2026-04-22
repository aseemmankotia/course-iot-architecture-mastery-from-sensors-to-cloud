## Chapter 12: Platforms That Scale — Quick Reference

### Core Concepts
| Concept | One-line explanation |
|---------|---------------------|
| Pooled Tenancy | All tenants share same resources (databases, compute); lowest cost, highest isolation risk |
| Siloed Tenancy | Each tenant gets dedicated infrastructure; maximum isolation, highest cost |
| Bridge Tenancy | Hybrid approach mixing shared and dedicated components based on tier/compliance needs |
| Tenant-scoped Partitioning | Data separation strategy using tenant IDs, schemas, or databases to enforce boundaries |
| Extension Points | Hooks allowing tenants to customize behavior without modifying core platform code |
| Cost Attribution | Tracking and allocating infrastructure costs per tenant for billing accuracy |
| Metering | Measuring tenant resource consumption (API calls, storage, compute) in real-time |

### Key Syntax / Commands
```
# Tenant-scoped query pattern
SELECT * FROM sensor_data WHERE tenant_id = :tenant_id AND device_id = :device_id;

# Row-level security policy (PostgreSQL)
CREATE POLICY tenant_isolation ON iot_events
  USING (tenant_id = current_setting('app.current_tenant')::uuid);

# Partition key design: tenant_id + timestamp for time-series IoT data
```

### Common Patterns
**Pattern 1: Noisy Neighbor Prevention**
Implement per-tenant rate limits and resource quotas; use separate queues/pools for high-volume tenants.

**Pattern 2: Tenant Context Propagation**
Inject tenant_id at API gateway; propagate through all service calls via headers/tokens for consistent scoping.

### Things to Remember
✅ Always enforce tenant isolation at the data layer, not just application logic
✅ Design metering from day one—retrofitting is expensive and error-prone
✅ Use compliance tier to drive tenancy model (regulated = siloed)
❌ Never trust client-provided tenant_id—validate against authenticated session

### Quick Quiz
1. When should you choose siloed over pooled? → When compliance (HIPAA, SOC2) requires guaranteed isolation or tenants demand dedicated resources
2. What's the biggest risk of pooled tenancy? → Cross-tenant data leakage from missing/broken tenant_id filters