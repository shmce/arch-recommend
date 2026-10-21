# arch-recommend

An architecture and technology decision skill for Codex.

Provides structured, decision-ready evaluations for software engineering, system design, data architecture, and technology stack choices.

## Features

- **Decisive Top Pick:** Delivers an opinionated default recommendation tailored to your constraints and existing stack.
- **Side-by-Side Comparison Matrix:** Evaluates candidates across operational complexity, latency/throughput, ecosystem maturity, maintenance overhead, and reversibility.
- **Graceful Stack Fallbacks:** If a chosen tool doesn't cleanly integrate with your current language/runtime, it proactively outlines interoperability bridges or viable alternatives.
- **One-Way vs. Two-Way Door Analysis:** Distinguishes high-switching-cost architectural commitments from easily swappable components.
- **Repository-Ready ADR:** Generates a copy-pasteable Architecture Decision Record formatted for your repository's `docs/adr/` folder.
- **Domain Guides:** Includes deep-dive reference guides for databases/storage engines, messaging/event streaming, and inter-service network APIs.

## Installation

Clone or install this skill into your Codex skills directory:

```bash
git clone https://github.com/shmce/arch-recommend.git ~/.codex/skills/arch-recommend
```

## Usage

This skill is designed for explicit invocation. Invoke it directly in Codex whenever you face an architectural fork in the road:

```text
$arch-recommend We are building a high-throughput event ingestion pipeline in Go. Should we use Kafka, AWS SQS, or RabbitMQ?
```

## Structure

```text
arch-recommend/
├── SKILL.md                          # Core instructions and analysis workflow
├── agents/
│   └── openai.yaml                   # Skill metadata and invocation policy
└── references/
    ├── adr_template.md               # Standard Architecture Decision Record format
    ├── api_communication.md          # REST vs. gRPC vs. GraphQL vs. tRPC guide
    ├── database_selection.md         # SQL vs. NoSQL vs. OLAP & engine trade-offs
    └── messaging_event_driven.md     # Queues vs. pub/sub vs. log streaming guide
```
