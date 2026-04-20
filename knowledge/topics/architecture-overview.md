---
title: Architecture Overview
type: topic
status: active
tags: [trading-system, architecture]
created: 2026-04-19
updated: 2026-04-19
---

# Architecture Overview

The system should be a modular monolith with layered intelligence. This means one coherent codebase, one domain model, and strong internal module boundaries, not business microservices.

## Design Target

The architecture is:

structured discretionary trading + deterministic discipline + AI-assisted context awareness

Rules protect process and capital. Context intelligence improves judgment. AI should assist with monitoring and interpretation, not become an unbounded decision maker.

## Major Layers

1. Deterministic Control Layer: rules, risk constraints, lifecycle enforcement, execution approval, and mandatory reviews.
2. Market and Context Observation Layer: market data, options data, filings, news, peers, macro, policy, sector behavior, and broker facts.
3. Context Intelligence Layer: thesis monitoring, regime detection, context comparison, contradiction detection, and change assessment.
4. Decision Support Layer: alerts, review prioritization, and decision-relevant outputs.

## Blueprint Scope

The application repo `DOCS/systems-blueprint.md` v2 defines logical system behavior, not the folder structure. Its layer and module names describe responsibilities that should be mapped into the runtime package under:

```text
src/trading_system/
```

The current mapping is:

- deterministic control: `domain`, `services`, and `rules_engine`
- observation: future modules
- context intelligence: future modules
- decision support: future modules

## Logical Module Areas

Logical responsibilities include:

- trading and planning
- execution
- rules engine
- context system
- analytics
- UI

These names should not be copied directly into folders unless the implementation pressure justifies them.

## Runtime Direction

Early development should run the Python app locally with `uv`, while Postgres can run in Docker. Full app, database, and worker containerization can come later if it becomes useful.

The implementation should begin with the [[first-vertical-slice]]: a planned discretionary swing trade from idea to review. This proves the lifecycle, deterministic rules, persistence, and review loop before expanding into watchlists, AI context ingestion, broker integration, or market data.

## Application Structure

The runtime codebase should be a Python modular monolith with explicit responsibility boundaries:

- `domain` for business concepts
- `services` for use-case orchestration
- `rules_engine` for deterministic rule evaluators
- `ports` for repository, unit-of-work, and event-bus interfaces
- `infrastructure` for persistence and runtime plumbing
- `app` for CLI and API input/output

See [[application-project-structure]] for the detailed boundary and import guidance.

## Related Pages

- [[deterministic-rules-vs-contextual-intelligence]]
- [[context-intelligence-layer]]
- [[development-workflow]]
- [[first-vertical-slice]]
- [[application-project-structure]]
