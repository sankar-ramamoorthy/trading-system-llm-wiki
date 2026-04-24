---
title: Issue 9 Durable JSON Persistence
type: raw-note
status: captured
tags: [trading-system, issue-9, persistence, milestone-2]
created: 2026-04-22
---

# Issue 9 Durable JSON Persistence

## Context

Issue 9 started Milestone 2 with durable persistence. The goal was to replace the
in-memory demo repositories so the core Milestone 1 objects survive across runs:

- `TradeIdea`
- `TradeThesis`
- `TradePlan`
- `Position`
- `Fill`
- `TradeReview`
- `LifecycleEvent`

The implementation also persists `RuleEvaluation` and `Violation` because the
existing demo workflow already creates them and repository ports already exist.

This work followed the application repository rules:

- keep the modular monolith boundary
- keep domain models independent from infrastructure
- keep services orchestrating through repository ports
- avoid broker integration, market data ingestion, AI, dashboards, and broad
  CLI redesign
- tie implementation to an explicit issue

Relevant application repo path:

```text
C:\Users\bosto\dockerstuff\trading-system
```

## Decisions Taken

Durable persistence backend:

- Chosen: local JSON file persistence.
- Deferred: SQLite, Postgres, Alembic migrations, and full SQLAlchemy repository
  behavior.
- Reason: JSON is the smallest useful durable step, preserves local simplicity,
  and proves persistence without introducing database setup or migration work.

CLI scope:

- Chosen: persist the existing `demo-planned-trade` workflow only.
- Deferred: create/show/list CRUD commands and query workflows.
- Reason: Issue 9 should prove durable storage first. Practical retrieval and CLI
  usability belong to later Milestone 2 issues.

Runtime data location:

- Default path: `.trading-system/store.json`
- Override: `TRADING_SYSTEM_STORE_PATH`
- `.trading-system/` is ignored by Git so local trading records are not committed.

Storage shape:

- One JSON document with top-level collections keyed by entity type.
- Entity records are keyed by UUID string.
- UUIDs, datetimes, and decimals are encoded as strings.
- Missing store file creates an empty store lazily.
- Invalid JSON raises a clear infrastructure error instead of silently replacing
  the file.

Write behavior:

- Writes use a same-directory temporary file and replace the target file.
- This keeps the implementation simple while avoiding partial file writes in the
  common case.

## What Was Implemented

Application repo changes:

- Added `src/trading_system/infrastructure/json/`.
- Added JSON-backed repository implementations for:
  - trade ideas
  - trade theses
  - trade plans
  - positions
  - fills
  - trade reviews
  - lifecycle events
  - rule evaluations
  - violations
- Added `build_json_repositories(path)` to create a repository set over one
  shared JSON store.
- Updated `demo-planned-trade` to use JSON repositories instead of in-memory
  repositories.
- Updated the demo output to print the JSON store path.
- Fixed the demo output to reload the persisted `Position` after fills, because
  durable repositories return fresh object instances rather than mutating the
  original object held by the CLI.
- Updated `.gitignore` to ignore `.trading-system/`.
- Updated `README.md` and `DOCS/milestone-2-roadmap.md` to record local JSON as
  the first durable persistence step.

Test coverage added:

- `tests/test_json_persistence.py`
- Verifies plan approval survives repository reload.
- Verifies position state, fills, review, and lifecycle details survive reload.
- Verifies the full Milestone 1 workflow persists rule artifacts.
- Verifies invalid JSON raises `JsonPersistenceError`.
- Verifies fill decimal and datetime fields round-trip.

Validation run:

```powershell
uv run pytest
```

Result:

```text
33 passed
```

CLI validation run:

```powershell
uv run trading-system demo-planned-trade
```

Result:

- command completed successfully
- `.trading-system/store.json` was created
- rerunning the demo accumulates additional demo records in the ignored local
  store
- final demo output showed the position closing correctly after the JSON-backed
  persistence fix

## Important Notes

This is not a final database architecture. It is a deliberately small local
durability step.

The existing SQLAlchemy/Postgres skeleton remains present but unused for Issue 9.
It can be revisited later when the project needs stronger database behavior,
schema migrations, concurrent access, richer querying, or deployment parity.

The next likely Milestone 2 work should be one of:

- query and retrieval workflows over persisted data
- practical CLI commands beyond the demo
- `OrderIntent`
- basic realized P&L from fills

The JSON store now gives those future issues real persisted data to work with.
