## Chapter 13: Architecting for the Real World — Quick Reference

### Core Concepts
| Concept | One-line explanation |
|---------|---------------------|
| Architecture Decision Records (ADRs) | Documented records capturing why specific technical decisions were made, including context and consequences |
| Tradeoff Documentation | Systematic recording of what you gained vs. sacrificed in each architectural choice |
| Failure Mode Analysis | Methodology for identifying how systems can fail and planning mitigations before deployment |
| Cost Projection | Forecasting infrastructure and operational expenses as IoT deployments scale |
| Capacity Planning | Estimating resource needs (compute, storage, bandwidth) for current and future device loads |
| Stakeholder Communication | Translating technical decisions into business impact language for non-technical audiences |

### Key Syntax / Commands
```
ADR Template:
# ADR-[number]: [Title]
## Status: [Proposed | Accepted | Deprecated | Superseded]
## Context: [What is the issue motivating this decision?]
## Decision: [What is the change being proposed?]
## Consequences: [What becomes easier/harder after this?]
## Alternatives Considered: [What else was evaluated?]
```

### Common Patterns
**Pattern 1: FMEA (Failure Mode and Effects Analysis)**
Rate each failure mode by Severity × Occurrence × Detection to prioritize risks

**Pattern 2: Decision Matrix**
Score options against weighted criteria (cost, latency, reliability, maintainability)

### Things to Remember
✅ Always document the "why" — code shows what, ADRs explain reasoning
✅ Quantify tradeoffs with actual numbers (latency, cost, uptime percentages)
✅ Update ADRs when decisions are revisited or superseded
❌ Don't assume stakeholders understand technical jargon — translate to business outcomes

### Quick Quiz
1. What survives personnel changes better than tribal knowledge? → **Written ADRs**
2. What three factors does FMEA multiply to prioritize risks? → **Severity × Occurrence × Detection**