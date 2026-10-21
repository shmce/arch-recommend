# Architecture Decision Record (ADR) Standards

When generating the ADR output, use this concise, battle-tested format. It should be directly copy-pasteable into `docs/adr/YYYY-MM-DD-<title>.md`.

## Markdown Format

```markdown
# ADR [Number]: [Descriptive Title of Decision]

## Status
[Proposed | Accepted | Superseded]

## Context & Problem Statement
* What is the core business or technical problem?
* What constraints exist (scale, latency, budget, existing tech stack)?
* What happens if we do nothing?

## Decision Drivers
* [Driver 1, e.g. Sub-50ms p99 latency under 20k RPS]
* [Driver 2, e.g. Minimal operational overhead for a 4-person team]
* [Driver 3, e.g. Seamless integration with existing Postgres / Node.js stack]

## Considered Options
* **Option 1 (Chosen):** [Name]
* **Option 2:** [Name]
* **Option 3:** [Name]

## Decision Outcome
Chosen option: **[Option 1]**, because [primary justification linked directly to decision drivers].

### Positive Consequences
* [Benefit 1]
* [Benefit 2]

### Negative Consequences & Trade-offs
* [Known downside, limitation, or operational cost accepted]

## Mitigation & Guardrails
* [How will the negative consequences or failure modes be monitored or mitigated?]

## One-Way vs. Two-Way Door Analysis
* **Reversibility Rating:** [Two-Way Door (Low switching cost) | One-Way Door (High switching cost / Data migration required)]
* **Exit Strategy:** [Brief path to migrate or pivot if scale/requirements change 18-24 months out]
```
