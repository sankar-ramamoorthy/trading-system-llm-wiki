---
title: Application Project Structure
type: topic
status: active
tags: [trading-system, architecture, python, project-structure]
created: 2026-04-19
updated: 2026-04-20
---

# Application Project Structure

The runtime application should start as a Python modular monolith that makes the [[first-vertical-slice]] easy to build without locking in premature abstractions.

## Runtime Shape

Use local Python development with Postgres in Docker:

- package name: `trading_system`
- dependency and environment manager: `uv`
- database: Postgres
- migrations: Alembic
- ORM: SQLAlchemy 2.x
- driver: `psycopg`
- first interface: Typer CLI
- API: FastAPI only when needed
- tests: pytest

## Top-Level Application Folders

Observed runtime structure in the application repo:

```text
trading-system/
|-- pyproject.toml
|-- scripts/
|-- tests/
`-- src/
    `-- trading_system/
        |-- app/
        |-- domain/
        |-- services/
        |-- rules_engine/
        |-- ports/
        `-- infrastructure/
```

Earlier guidance included `docker-compose.yml`, Alembic, migrations, and `bootstrap/`. Those remain plausible later additions, but they are not required to describe the current implementation state.

## Responsibility Boundaries

`domain/` holds pure business concepts and should not know about SQLAlchemy, FastAPI, Typer, or database sessions.

`services/` orchestrates use cases, workflow order, transaction boundaries, rule execution, and lifecycle event emission.

`rules_engine/` contains deterministic Python rule evaluators. Do not build a rule DSL until the actual abstractions are visible.

`ports/` defines repository, unit-of-work, and event-bus interfaces.

`infrastructure/` implements persistence, database sessions, SQLAlchemy models, repository implementations, and runtime plumbing.

`app/` owns CLI and API input/output only. It should not contain business logic.

## Import Direction

Keep imports mostly one-way:

```text
app -> services -> domain
services -> ports
infrastructure -> ports + domain
rules_engine -> domain
domain -> domain only
```

Avoid dependencies from `domain` to `infrastructure`, from `domain` to `app`, and from `rules_engine` to `app`.

## Milestone One Status

Milestone 1 is complete as a local CLI-driven MVP. Current implementation includes enough structure to support:

- domain objects for idea, thesis, plan, position, fill, review, rule evaluation, violation, and lifecycle event
- services for plan approval, rule evaluation, position management, fill recording, and review creation
- a small rules engine with concrete Python evaluators
- in-memory repositories for local workflow tests and demo execution
- SQLAlchemy infrastructure skeleton
- a CLI command surface for the canonical planned-trade demo

Milestone 1 deliberately leaves these to later work:

- persistence behavior proven through infrastructure adapters
- practical CLI commands beyond the demo
- `OrderIntent`
- basic P&L
- broker, market data, API, UI, analytics, and AI integrations

## Current Commands

The updated app repo README records these local commands:

```powershell
uv run pytest
uv run trading-system version
uv run trading-system demo-planned-trade
```

## Deliberate Non-Choices

Avoid these until later milestones justify them:

- generic `utils/` or `shared/` dumping grounds
- event sourcing
- message brokers
- broker adapters
- AI packages
- generic plugin frameworks for rules
- a full API surface before the CLI and persistence workflows prove the use cases

## Related Pages

- [[architecture-overview]]
- [[first-vertical-slice]]
- [[development-workflow]]
- [[application-implementation-status]]
- [[milestone-2-roadmap]]
