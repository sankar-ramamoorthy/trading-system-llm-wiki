---
title: Issue 10 Retrieval Context And Decisions
type: raw-note
status: captured
tags: [trading-system, issue-10, retrieval, cli, milestone-2]
created: 2026-04-22
---

# Issue 10 Retrieval Context And Decisions

## Context

Issue 10 followed Issue 9's local JSON persistence work.

Issue 9 made the Milestone 1 demo durable, but the application still lacked a
normal way to inspect persisted records. The only practical way to see stored
data was to open `.trading-system/store.json` directly.

Issue 10 addressed that gap with read-only retrieval workflows.

## Decision Summary

Chosen next issue:

- Issue 10: retrieval workflows for persisted positions.

Reason:

- Persistence is much more useful once persisted records can be listed and
  inspected from the CLI.
- Retrieval gives later work real visibility into stored data before changing the
  domain with `OrderIntent`.
- This is a smaller, safer step than introducing `OrderIntent`, P&L, or broader
  create/update CLI commands immediately after persistence.

Explicitly deferred:

- `OrderIntent`
- basic realized P&L
- practical create/update CLI commands
- richer querying/filtering
- dashboards, APIs, broker integration, market data, or automation

## Implemented Behavior

The app gained read-only CLI commands backed by the same JSON store path used by
the durable demo:

```powershell
uv run trading-system list-positions
uv run trading-system list-positions --state closed
uv run trading-system show-position <position-id>
uv run trading-system show-position-timeline <position-id>
```

`list-positions` shows persisted positions with:

- position id
- lifecycle state
- purpose
- current quantity
- opened timestamp
- closed timestamp

`show-position` shows:

- position summary
- linked trade plan
- linked trade idea
- fills for the position, ordered by fill time
- review summary when present

`show-position-timeline` shows lifecycle events for the position ordered by
`occurred_at`.

Invalid UUIDs and missing positions are reported as clear Typer command errors.

## Architecture Notes

The implementation kept the intended modular monolith boundaries:

- repository ports define the retrieval capabilities
- in-memory and JSON repositories implement the same read methods
- a read-only `PositionQueryService` composes repositories into retrieval views
- the CLI formats output and delegates retrieval behavior to the service
- domain entities remain unchanged

Repository ports were extended with minimal read methods:

- `PositionRepository.list_all()`
- `FillRepository.list_by_position_id(position_id)`
- `LifecycleEventRepository.list_by_entity(entity_type, entity_id)`

`TradeReviewRepository.get_by_position_id(position_id)` already existed and was
reused.

## Test And Validation Notes

New or expanded tests covered:

- repository read methods over JSON persistence
- service-level retrieval using in-memory repositories
- CLI retrieval using `tmp_path` and `TRADING_SYSTEM_STORE_PATH`
- filtering closed positions
- showing fills and review for a position
- showing lifecycle timelines in order
- invalid UUID and missing-position command errors

Validation result:

```text
uv run pytest
44 passed
```

Manual CLI smoke checks were run successfully:

```powershell
uv run trading-system demo-planned-trade
uv run trading-system list-positions
uv run trading-system list-positions --state closed
uv run trading-system show-position <position-id>
uv run trading-system show-position-timeline <position-id>
```

One sandbox-related note: some `uv run trading-system ...` retrieval commands
needed to be rerun outside the sandbox because `uv` hit an access-denied error
against its local cache directory. The commands themselves passed after rerun.

## Follow-Up Implications

After Issue 10, the project has both:

- durable JSON storage
- read-only retrieval from that storage

This makes the next Milestone 2 issue easier to choose and validate.

Likely next candidates:

- `OrderIntent`, because persisted and inspectable trade records now exist
- practical create/update CLI commands, if manual day-to-day use should come
  before new domain modeling
- basic realized P&L, if closed-position financial feedback is more urgent

The strongest domain-first next step is probably `OrderIntent`, because the
system can now inspect persisted positions and fills before adding the missing
intent layer between `TradePlan` and actual `Fill`.
