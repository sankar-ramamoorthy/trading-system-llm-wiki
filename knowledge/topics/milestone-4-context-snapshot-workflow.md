---
title: Milestone 4 Context Snapshot Workflow
type: topic
status: active
tags: [trading-system, milestone-4, market-context, snapshots]
created: 2026-04-26
updated: 2026-04-26
---

# Milestone 4 Context Snapshot Workflow

The first Milestone 4 implementation slice uses local context snapshot import before any external market-data provider.

This keeps market context read-only, auditable, local-first, and visibly separate from canonical trade records.

## Implemented Initial Slice

The implemented workflow imports a local JSON context document, stores it as an immutable `MarketContextSnapshot`, and makes it inspectable from the CLI.

A snapshot is tied to an `instrument_id` and may optionally link to a `TradePlan`, `Position`, or `TradeReview`.

The snapshot can inform planning and review, but it must not change:

- trade meaning
- thesis or plan content
- rule evaluations
- approvals
- fills
- position state
- review conclusions

## Snapshot Shape

The domain record is `MarketContextSnapshot`.

Useful fields:

- `id`
- `instrument_id`
- optional `target_type`
- optional `target_id`
- `context_type`
- `source`
- optional `source_ref`
- `observed_at`
- `captured_at`
- flexible JSON `payload`

The payload should stay flexible for the first slice, while the envelope fields should be validated.

Example import shape:

```json
{
  "context_type": "price_snapshot",
  "observed_at": "2026-04-26T16:00:00-04:00",
  "payload": {
    "symbol": "AAPL",
    "last": "185.25",
    "notes": "Delayed close snapshot"
  }
}
```

## Source Boundary

The first source is file import, not a live provider.

Reasons:

- fits the local-first and auditable project direction
- avoids credentials, network failures, rate limits, and upstream API churn
- keeps tests deterministic
- forces a clean model for storing context as captured snapshots

The implementation defines a source/provider port so later providers can plug in without changing the service and repository shape.

## Implementation Components

The implementation boundary includes:

- `MarketContextSnapshotRepository` for adding, retrieving, and listing snapshots
- `MarketContextImportSource` for loading validated snapshot input from a source
- JSON persistence collection `market_context_snapshots`
- in-memory repository support for tests
- local JSON-file import adapter for the first source
- `MarketContextImportService` for validation, target linkage, instrument derivation, and snapshot storage
- `MarketContextQueryService` for snapshot detail and list workflows

When a snapshot links to a target, the import service validates that the target exists. If possible, it derives `instrument_id` from the linked `TradePlan`, `Position`, or `TradeReview`; otherwise an explicit `instrument_id` is required.

## External Provider Deferral

`yfinance` or another external provider may be useful later as a prototype-grade, read-only snapshot source.

It should not be the first implementation path.

If introduced later, provider data should be:

- optional
- advisory
- non-canonical
- immediately stored as an auditable snapshot
- isolated behind a provider/source boundary

Provider failures must not block the core trade workflow.

An ADR should be introduced when adding `yfinance` or any external data provider. That ADR should record that provider data is advisory, snapshot-based, non-streaming, and never allowed to trigger execution or automated plan creation.

## CLI Direction

Implemented CLI surface:

```text
import-context <file> [--instrument-id UUID] [--target-type trade-plan|position|trade-review] [--target-id UUID] [--source NAME]
list-context --instrument-id UUID
list-context --target-type trade-plan|position|trade-review --target-id UUID
show-context <snapshot-id>
```

The CLI should continue to make the boundary obvious: context is shown alongside planning or review data, not merged into canonical trade meaning.

## Testing Expectations

Verified behavior:

- import stores an immutable snapshot with `captured_at`
- invalid or corrupt import JSON returns a clear error
- JSON persistence round-trips snapshots after reload
- snapshots list by instrument
- snapshots list by target
- target validation failures are explicit
- linked-target imports derive or validate the correct instrument
- importing context does not mutate canonical trade records

Focused market-context tests passed on 2026-04-26:

```text
uv run pytest tests\test_market_context_service.py tests\test_market_context_persistence.py tests\test_cli_market_context.py
13 passed
```

The full application suite also passed:

```text
uv run pytest
117 passed
```

## Related Pages

- [[milestones-3-to-5-roadmap]]
- [[context-intelligence-layer]]
- [[deterministic-rules-vs-contextual-intelligence]]
- [[data-and-platform-strategy]]
