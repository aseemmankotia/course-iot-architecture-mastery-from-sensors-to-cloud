## Chapter 13: Architecting for the Real World — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | What is an Architecture Decision Record (ADR)? | A document that captures an important architectural decision along with its context, consequences, and the reasoning behind the choice |
| 2 | What key skill separates engineers from architects according to this chapter? | The ability to make defensible tradeoff decisions and communicate them in ways that survive personnel changes and persuade stakeholders |
| 3 | What are the four essential sections of a well-structured ADR? | Context, Decision, Consequences, and Status (with optional Alternatives Considered) |
| 4 | Why is tradeoff documentation critical in IoT architecture? | It preserves institutional knowledge, justifies decisions to stakeholders, and prevents revisiting resolved debates when team members change |
| 5 | What is Failure Mode Analysis (FMA) in IoT systems? | A systematic methodology for identifying potential points of failure, their likelihood, impact, and mitigation strategies |
| 6 | What three factors does cost projection typically account for in IoT deployments? | Device/hardware costs, connectivity/bandwidth costs, and cloud compute/storage costs at scale |
| 7 | What is capacity planning in IoT architecture? | The process of determining infrastructure requirements to handle expected device growth, data volumes, and peak loads over time |
| 8 | How should technical decisions be communicated to non-technical stakeholders? | By focusing on business impact, risk mitigation, cost implications, and outcomes rather than implementation details |
| 9 | What is the purpose of documenting rejected alternatives in an ADR? | To show due diligence, prevent re-litigation of decisions, and explain why seemingly viable options weren't chosen |
| 10 | What does "defensible" mean in the context of architectural decisions? | The decision can be justified with evidence, analysis, and clear reasoning that addresses anticipated objections |
| 11 | What are common failure modes specific to IoT edge devices? | Network disconnection, power loss, sensor drift/malfunction, memory exhaustion, and firmware corruption |
| 12 | Why should ADRs be version-controlled alongside code? | They evolve with the system, provide historical context, and ensure decisions are discoverable by future team members |
| 13 | What is the difference between CAPEX and OPEX in IoT cost planning? | CAPEX is upfront capital expenditure (devices, installation); OPEX is ongoing operational costs (connectivity, cloud services, maintenance) |
| 14 | How does failure mode analysis inform architecture decisions? | By revealing which components need redundancy, which failures are acceptable, and where monitoring is critical |
| 15 | What communication technique helps stakeholders understand technical tradeoffs? | Using analogies, visualizations, and framing choices in terms of business priorities like cost, time-to-market, and reliability |

### Key Terms

| Term | Definition |
|------|-----------|
| Architecture Decision Record (ADR) | A lightweight document format for capturing significant architectural decisions and their rationale |
| Tradeoff Analysis | The systematic evaluation of competing factors (cost, performance, complexity) when making design choices |
| Failure Mode Analysis | A methodology for identifying how system components can fail and planning appropriate mitigations |
| Capacity Planning | Forecasting resource requirements to ensure systems can handle projected growth and peak demands |
| Technical Debt | The implied cost of future rework caused by choosing expedient solutions over better long-term approaches |
| Stakeholder Communication | The practice of translating technical concepts into business-relevant terms for decision-makers |
| Decision Survivability | The quality of documentation that allows decisions to remain understood after original authors leave |

### Memory Tricks
- **ADR = "Always Document Rationale"** — reminds you that the reasoning matters as much as the decision itself
- **FMA = "Find Murphy's Attacks"** — failure mode analysis is about finding everything that could go wrong before Murphy's Law strikes
- **COST planning: Compute, Operations, Scale, Time** — the four dimensions to consider when projecting IoT expenses
- **The "Bus Factor" test** — if you got hit by a bus, would your ADRs let someone else understand why the system was built this way?