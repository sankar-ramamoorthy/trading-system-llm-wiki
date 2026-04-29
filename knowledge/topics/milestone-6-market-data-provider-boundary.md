---
title: Milestone 6 Market Data Provider Boundary
type: topic
status: complete
tags: [trading-system, milestone-6, market-data, providers, yfinance, massive]
created: 2026-04-27
updated: 2026-04-29
---

# Milestone 6 Market Data Provider Boundary

Milestone 6 completed read-only external market data provider integration.

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
4. treat Massive.com provider planning as completed Milestone 6C Issue 1
5. treat Massive.com daily bars implementation as completed Milestone 6C Issue 2
6. close Milestone 6 before beginning the API-first web product implementation from ADR-008

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
- yfinance remains the default provider
- validation on 2026-04-27: 10 focused provider-boundary tests passed and 166 full-suite tests passed

## Massive.com Provider

Massive.com is accepted as the next provider candidate after yfinance.

ADR-009 records the Massive.com boundary:

- official `massive` Python client is the preferred first implementation path
- `MASSIVE_API_KEY` is the initial credential boundary
- daily aggregate/OHLCV-style bars are the first data shape
- provider output remains stored as `MarketContextSnapshot`
- yfinance remains the default provider until a later decision changes it

Initial Massive.com implementation should preserve the same boundary:

- read-only data only
- daily OHLCV or daily aggregate bars first
- explicit user-invoked fetches
- stored `MarketContextSnapshot` output
- no live streaming, recommendations, broker integration, execution triggers, automatic refresh, or domain mutation
- credentials handled outside snapshots and committed files

## Completed Slice: Milestone 6C Issue 1

Massive.com provider planning is complete.

- ADR-009 is accepted in the application repo
- no Massive.com dependency or runtime adapter was added
- the follow-on implementation target was a narrow Massive.com daily bars adapter behind the provider registry

## Completed Slice: Milestone 6C Issue 2

Massive.com daily bars are implemented behind the provider registry.

- `fetch-market-data` now accepts `--provider massive`
- Massive fetches require `MASSIVE_API_KEY`
- the official `massive` Python client is used through an infrastructure adapter
- daily aggregate bars are normalized into application-owned `daily_ohlcv` snapshots
- snapshots use `source = "massive"`
- yfinance remains the default provider
- no fallback exists between providers
- validation on 2026-04-29: 21 focused market-data tests passed and 177 full-suite tests passed

The implementation closeout is recorded in the application repo at:

```text
C:\Users\bosto\dockerstuff\trading-system\DOCS\milestone-6c-massive-daily-bars-closeout.md
```

## Current Decision Point

Milestone 6D is complete. The market-data provider milestone is closed.

The next accepted implementation direction is ADR-008 API-first web product and trade-capture draft workflow. A narrow API-key ergonomics issue can still be considered first, but only if it stays operational and local; it should not introduce cloud secret management, user accounts, provider fallback, or web-product configuration scope.

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
- [[milestone-6c-massive-daily-bars-implemented-20260429]]
- [[milestone-6-closeout-20260429]]
