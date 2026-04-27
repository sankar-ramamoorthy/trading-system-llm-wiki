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

The first implementation issue, Milestone 6A, is now complete: explicit user-invoked `fetch-market-data` fetch-and-store behavior for daily OHLCV snapshots.

Provider response objects and schemas should stay inside the adapter boundary. The rest of the application should use stored snapshots and existing context inspection workflows.

The accepted sequencing after the API-first product brainstorm is:

1. finish Milestone 6 before starting ADR-008 implementation
2. treat the completed yfinance slice as Milestone 6A
3. treat provider-boundary hardening as completed Milestone 6B Issue 1
4. plan Massive.com, formerly Polygon.io, as the next provider candidate
5. close or explicitly pause Milestone 6 before beginning the API-first web product implementation from ADR-008

## Completed Slice: Milestone 6A

- `fetch-market-data <symbol> --start YYYY-MM-DD --end YYYY-MM-DD`
- optional `--instrument-id`, `--target-type`, and `--target-id` linking
- `daily_ohlcv` snapshot payloads with raw OHLCV plus adjusted close
- validation on 2026-04-27: 6 focused yfinance tests passed and 162 full-suite tests passed

## Completed Slice: Milestone 6B Issue 1

Provider-boundary hardening is complete.

- `fetch-market-data` now accepts explicit `--provider yfinance`
- existing calls without `--provider` still default to yfinance
- CLI code resolves provider-backed source metadata through a provider registry
- unsupported provider names fail clearly
- yfinance remains the only implemented provider
- validation on 2026-04-27: 10 focused provider-boundary tests passed and 166 full-suite tests passed

## Next Provider Candidate

Massive.com is the next provider candidate to plan after yfinance.

Initial Massive.com planning should preserve the same boundary:

- read-only data only
- daily OHLCV or daily aggregate bars first
- explicit user-invoked fetches
- stored `MarketContextSnapshot` output
- no live streaming, recommendations, broker integration, execution triggers, automatic refresh, or domain mutation
- credentials handled outside snapshots and committed files

This may become a companion ADR or ADR-007 update if it changes provider status, credential rules, fallback behavior, or supported data shapes.

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
