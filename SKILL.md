---
name: arch-recommend
description: Evaluates software engineering, systems design, and architectural options. Produces a side-by-side comparison matrix, an opinionated top recommendation, trade-offs, failure modes, and a repository-ready ADR when explicitly invoked with $arch-recommend.
---

# Architecture & Tech Recommender

Provide structured, decision-ready evaluations for software engineering, system design, data architecture, and technology stack choices.

## Invocation & Purpose

This skill is invoked explicitly (e.g. `$arch-recommend` or direct requests to evaluate architecture/tools) when deciding between libraries, patterns, database engines, cloud services, communication protocols, or system topologies.

## Core Evaluation Principles

1. **Stack Compatibility with Graceful Fallback**
   * First, check if the choice integrates natively with the user's existing tech stack and runtime environment.
   * If the user's existing stack cannot cleanly support the ideal solution (e.g. library missing SDK, runtime incompatibility, or excessive friction), **explicitly call out the friction and propose the best interoperable alternatives** (e.g., sidecar proxy, standalone microservice, webhook/REST bridge, or an alternative tool native to their language).

2. **Decisive, Opinionated Default**
   * Never sit on the fence or offer a list without a clear default pick.
   * Lead with the top recommendation, followed by clear pivot rules: "Choose Option B if [specific condition]".

3. **Reversibility & Trapdoors**
   * Classify choices as **Two-Way Doors** (swappable library, stateless protocol adapter) vs. **One-Way Doors** (core database engine, log-based event broker format, high data-migration lock-in).
   * Identify day-1 vs. day-1000 operational debt and failure modes.

## Reference Routing

Read relevant references when handling specific technical domains:

* **Databases, Caches & Storage Engines:** Read [references/database_selection.md](references/database_selection.md) for relational vs. NoSQL, B-Tree vs. LSM, and access patterns.
* **Brokers, Queues & Event Streaming:** Read [references/messaging_event_driven.md](references/messaging_event_driven.md) for queues vs. smart-broker vs. log streaming (Kafka/RabbitMQ/SQS/NATS).
* **Network & Inter-Service APIs:** Read [references/api_communication.md](references/api_communication.md) for REST vs. gRPC vs. GraphQL vs. tRPC.
* **Architecture Decision Record (ADR) Spec:** Read [references/adr_template.md](references/adr_template.md) for standard ADR formatting.

## Response Structure

Format every recommendation with the following four sections:

### 1. Decision Summary & Top Pick
* **Recommended Pick:** Direct statement of the winning candidate.
* **Core Rationale:** 2–3 sentences summarizing why it wins under the provided constraints.
* **Stack Fit:** How it connects to the user's stack (or the bridge/alternative if an impedance mismatch exists).

### 2. Side-by-Side Comparison Matrix
Construct a markdown comparison table evaluating 2 to 4 viable candidates:

| Dimension | [Option A (Recommended)] | [Option B] | [Option C] |
| :--- | :--- | :--- | :--- |
| Operational Complexity | Low (Managed/simple) | High (Cluster ops) | Medium |
| Latency & Throughput | [Quantitative / Qualitative] | [Quantitative / Qualitative] | [Quantitative / Qualitative] |
| Ecosystem & Stack Fit | Native support / Mature | Requires sidecar / bridge | Growing |
| Maintenance Overhead | Low | High ongoing patching | Moderate |
| Reversibility | Two-Way Door | One-Way Door | Two-Way Door |
| Best For... | [Ideal scenario] | [Alternative scenario] | [Niche scenario] |

### 3. Trade-offs, Pivot Rules & Failure Modes
* **When to Pivot:**
  * Choose **[Option B]** if: [concrete condition, e.g. sub-millisecond p99 SLA or self-hosted constraint].
  * Choose **[Option C]** if: [concrete condition].
* **Failure Modes & Operational Traps:**
  * **[Option A Trap]**: [what fails under load or when teams regret it].
  * **[Option B Trap]**: [known operational hazard or scaling cliff].

### 4. Repository-Ready Architecture Decision Record (ADR)
Provide a ready-to-commit ADR block following [references/adr_template.md](references/adr_template.md):

```markdown
# ADR: [Decision Title]
## Status: Proposed
## Context: [Brief problem statement and constraints]
## Decision: [Chosen candidate and key rationale]
## Consequences: [Positive gains vs accepted trade-offs]
## Reversibility & Exit Strategy: [Two-Way / One-Way door classification and pivot plan]
```
