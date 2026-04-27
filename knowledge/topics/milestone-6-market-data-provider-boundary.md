---
title: Milestone 6 Market Data Provider Boundary
type: topic
status: active
tags: [trading-system, milestone-6, market-data, providers, yfinance]
created: 2026-04-27
updated: 2026-04-27
---

# Milestone 6 Market Data Provider Boundary

Milestone 6 starts read-only external market data provider integration.

The application repo source of truth is:

```text
C:\Users\bosto\dockerstuff\trading-system\DOCS\ADR\007-market-data-provider-boundary.md
```

## Accepted Boundary

ADR-007 accepts the first provider boundary:

- `yfinance` may be used as an optional prototype-grade provider adapter.
- The first data shape is daily OHLCV history only.
- Provider output must be stored as explicit `MarketContextSnapshot` records.
- Provider data remains advisory and non-canonical.
- Provider failures must not block core trade workflows.

The application still owns trade meaning. External providers supply market facts with caveats.

## Implementation Direction

The first implementation issue is now complete: explicit user-invoked `fetch-market-data` fetch-and-store behavior for daily OHLCV snapshots.

Provider response objects and schemas should stay inside the adapter boundary. The rest of the application should use stored snapshots and existing context inspection workflows.

## Implemented Slice

- `fetch-market-data <symbol> --start YYYY-MM-DD --end YYYY-MM-DD`
- optional `--instrument-id`, `--target-type`, and `--target-id` linking
- `daily_ohlcv` snapshot payloads with raw OHLCV plus adjusted close

## Non-Scope

Milestone 6 should not introduce:

- live streaming market data
- execution-grade quotes
- broker integration
- execution triggers
- automatic refresh daemons
- provider-driven recommendations
- automatic trade, thesis, review, rule, or lifecycle mutation
- AI or ML interpretation of provider data

## Related Pages

- [[milestone-4-context-snapshot-workflow]]
- [[data-and-platform-strategy]]
- [[deterministic-rules-vs-contextual-intelligence]]
- [[application-implementation-status]]
- [[product-roadmap-and-learning-boundaries]]
