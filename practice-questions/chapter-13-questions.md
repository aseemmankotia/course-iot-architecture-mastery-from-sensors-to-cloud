## Chapter 13: Architecting for the Real World — Practice Questions

### Multiple Choice

**Q1.** What is the primary purpose of an Architecture Decision Record (ADR)?

A) To document the final implementation code for a system component
B) To capture the context, decision, and consequences of significant architectural choices
C) To provide a user manual for system operators
D) To track bug fixes and feature requests

<details>
<summary>Answer</summary>

**Correct: B**

ADRs are specifically designed to document the reasoning behind architectural decisions, including the context in which the decision was made, the decision itself, and its anticipated consequences. Option A describes code documentation, Option C describes operational documentation, and Option D describes issue tracking—none of which capture architectural decision-making.

</details>

---

**Q2.** When conducting failure mode analysis for an IoT system, which approach best identifies cascading failures?

A) Testing each component in isolation under normal operating conditions
B) Mapping dependencies and systematically analyzing what happens when each component fails
C) Only examining the most expensive components in the system
D) Reviewing marketing materials for each vendor's reliability claims

<details>
<summary>Answer</summary>

**Correct: B**

Failure mode analysis requires understanding system dependencies and examining how failures propagate through the system. Option A misses the interconnected nature of failures. Option C ignores that inexpensive components can cause critical failures. Option D relies on unverified claims rather than systematic analysis.

</details>

---

**Q3.** Which cost factor is most commonly underestimated in IoT capacity planning?

A) Initial hardware procurement
B) Software licensing fees
C) Data egress, storage growth, and ongoing operational costs
D) Marketing expenses

<details>
<summary>Answer</summary>

**Correct: C**

IoT systems generate continuous data streams that accumulate over time. Data egress charges, storage costs, and operational overhead often grow unpredictably and are frequently underestimated. Initial hardware (A) and licensing (B) are typically one-time or predictable costs. Marketing (D) is unrelated to technical capacity planning.

</details>

---

**Q4.** When presenting a tradeoff analysis to non-technical stakeholders, which approach is most effective?

A) Provide detailed technical specifications and let stakeholders interpret them
B) Frame tradeoffs in terms of business impact: cost, time-to-market, risk, and scalability
C) Recommend only one option without discussing alternatives
D) Use as much technical jargon as possible to demonstrate expertise

<details>
<summary>Answer</summary>

**Correct: B**

Non-technical stakeholders need to understand decisions in terms of business outcomes they care about. Option A assumes technical literacy they may lack. Option C prevents informed decision-making. Option D creates barriers to understanding rather than enabling communication.

</details>

---

**Q5.** In an ADR, the "Consequences" section should include:

A) Only the positive outcomes of the decision
B) Both positive and negative outcomes, including accepted tradeoffs
C) A list of team members who agreed with the decision
D) The budget approval documentation

<details>
<summary>Answer</summary>

**Correct: B**

The Consequences section provides a balanced view of what the decision means for the system, including benefits, drawbacks, and tradeoffs that were consciously accepted. Option A presents an incomplete picture. Options C and D are administrative details that belong elsewhere.

</details>

---

### True / False

**Q6.** An Architecture Decision Record should be written after the system is fully deployed to ensure accuracy. — **True / False**

**False**

*ADRs should be written at the time the decision is made, capturing the context and reasoning while they are fresh. Writing them after deployment risks losing important context about why certain tradeoffs were accepted, what alternatives were considered, and what constraints existed at decision time.*

---

**Q7.** Cost projections for IoT systems should account for variable pricing models used by cloud providers, which often scale non-linearly with usage. — **True / False**

**True**

*Cloud pricing structures include tiered rates, data transfer costs, and various surcharges that can create non-linear cost growth. Accurate capacity planning must model these pricing structures to avoid budget surprises as the system scales.*

---

### Short Answer

**Q8.** Describe three key components that should be included in every Architecture Decision Record.

<details>
<summary>Answer</summary>

1. **Context**: The circumstances, constraints, and forces that influenced the decision, including technical requirements, business needs, and existing system state.

2. **Decision**: A clear statement of the architectural choice that was made, including what approach was selected from available alternatives.

3. **Consequences**: The resulting outcomes of the decision, both positive and negative, including accepted tradeoffs, new capabilities enabled, and technical debt incurred.

Additional components often include Status (proposed, accepted, deprecated), Date, and links to related ADRs.

</details>

---

**Q9.** What is the difference between FMEA (Failure Mode and Effects Analysis) and FTA (Fault Tree Analysis) when analyzing IoT system reliability?

<details>
<summary>Answer</summary>

**FMEA** is a bottom-up approach that starts with individual components, identifies how each can fail, and traces the effects upward through the system. It systematically catalogs all potential failure modes.

**FTA** is a top-down approach that starts with an undesired system-level event (like complete system outage) and works backward to identify all possible causes and combinations of failures that could lead to that event.

FMEA is better for comprehensive component-level analysis, while FTA excels at understanding how multiple failures combine to cause critical system events. Many teams use both approaches together for thorough reliability analysis.

</details>

---

### Scenario-Based

**Q10.** You are architecting an IoT solution for a smart agriculture company that needs to monitor soil moisture across 500 acres using 2,000 sensors. The CTO wants a fully cloud-based solution for simplicity, but the CFO is concerned about long-term costs. The field operations manager worries about connectivity in rural areas with spotty cellular coverage. 

Describe how you would structure your architectural decision-making process, including: (a) what tradeoff documentation you would create, (b) how you would analyze potential failure modes, and (c) how you would present your recommendations to these three stakeholders with different concerns.

<details>
<summary>Answer</summary>

**(a) Tradeoff Documentation:**
Create ADRs for key decisions including:
- Cloud-only vs. edge/hybrid architecture (documenting latency, cost, and reliability tradeoffs)
- Connectivity strategy (cellular, LoRaWAN, satellite backup)
- Data aggregation strategy (raw data vs. edge-processed summaries)

For each ADR, document alternatives considered, selection criteria weighted by stakeholder priorities, and explicit tradeoffs accepted.

**(b) Failure Mode Analysis:**
Apply FMEA to identify:
- Sensor failures (battery depletion, physical damage, calibration drift)
- Connectivity failures (cellular outages, gateway failures)
- Cloud service outages and their impact on farming operations

Use FTA to analyze critical scenarios like "irrigation system makes wrong decision" by tracing back to sensor errors, connectivity gaps, or cloud processing failures.

Include cost-of-failure estimates (crop loss from missed irrigation vs. system cost).

**(c) Stakeholder Communication:**
- **CTO**: Present technical architecture diagrams, edge computing benefits for latency and reliability, and reduced cloud complexity through data aggregation
- **CFO**: Provide 3-year TCO projections comparing cloud-only vs. hybrid approaches, highlighting how edge processing reduces data egress costs by 60-80%
- **Field Operations**: Demonstrate offline capability, local data buffering during outages, and gateway redundancy ensuring no data loss during connectivity gaps

Create a unified recommendation that addresses all concerns: a hybrid architecture with edge gateways that reduces cloud costs, improves reliability, and handles connectivity issues—with clear metrics showing how each stakeholder's concerns are addressed.

</details>