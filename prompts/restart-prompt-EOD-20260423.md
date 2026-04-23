---
title: EOD Restart Prompt 2026-04-23
type: prompt
status: active
tags: [trading-system, restart, milestone-2]
created: 2026-04-23
---

# Restart Prompt EOD 2026-04-23

Use this prompt to restart work tomorrow.

```text
I am building a professional-grade personal trading system in Python using a modular monolith architecture.

Treat the following as established context. Do not re-litigate completed decisions unless I explicitly ask.

Application repo:
C:\Users\bosto\dockerstuff\trading-system

Knowledge base:
C:\Users\bosto\dockerstuff\knowledge-base\trading-system

Before coding, align with the app repo AGENTS.md and the knowledge-base notes.

Current state:

- Milestone 1 is complete.
- Milestone 2 has started.
- Issue 9 implemented local JSON durable persistence for the Milestone 1 workflow.
- Issue 10 implemented read-only retrieval workflows over the JSON store.
- The JSON store path defaults to `.trading-system/store.json`.
- `TRADING_SYSTEM_STORE_PATH` can override the store path.
- `.trading-system/` is ignored and may contain local demo records.

Implemented Issue 9:

- JSON-backed repositories under `src/trading_system/infrastructure/json/`
- durable persistence for TradeIdea, TradeThesis, TradePlan, Position, Fill, TradeReview, LifecycleEvent, RuleEvaluation, and Violation
- `demo-planned-trade` now writes durable JSON records

Implemented Issue 10:

- repository read methods for positions, fills, and lifecycle events
- `PositionQueryService` for read-only retrieval
- CLI commands:
  - `uv run trading-system list-positions`
  - `uv run trading-system list-positions --state closed`
  - `uv run trading-system show-position <position-id>`
  - `uv run trading-system show-position-timeline <position-id>`

Known validation:

- `uv run pytest` passed with 44 tests.
- The demo and retrieval commands passed in manual smoke checks.
- Some `uv run trading-system ...` commands may need permission outside sandbox if uv hits an access-denied error against its local cache.

Important constraints:

- Preserve the modular monolith boundaries.
- Domain must not depend on infrastructure, Typer, SQLAlchemy, or JSON.
- Services orchestrate workflows and retrieval; CLI formats input/output.
- Do not collapse TradeIdea, TradeThesis, TradePlan, Position, OrderIntent, Fill, RuleEvaluation, Violation, or TradeReview.
- Position must originate from an approved TradePlan, not directly from a TradeIdea.
- Keep broker integration, market data ingestion, AI, dashboards, and automation out of scope.

Useful knowledge-base raw notes:

- `knowledge/raw/issue-9-durable-json-persistence-20260422.md`
- `knowledge/raw/issue-10-retrieval-proposed-plan-20260422.md`
- `knowledge/raw/issue-10-retrieval-implemented-20260422.md`
- `knowledge/raw/issue-10-retrieval-context-and-decisions-20260422.md`

Likely next issue:

Start by confirming or challenging the next issue. The strongest domain-first candidate is `OrderIntent`, now that persisted records can be inspected. If practical day-to-day use is more urgent, consider create/update CLI commands before OrderIntent. If financial feedback is more urgent, consider basic realized P&L from fills.

When responding, be practical and opinionated. Prefer one issue at a time, tight scope, simple implementation, and strong domain boundaries.
```
