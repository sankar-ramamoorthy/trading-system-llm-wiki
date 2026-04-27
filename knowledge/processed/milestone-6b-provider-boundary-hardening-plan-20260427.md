---
title: Milestone 6B Provider Boundary Hardening Plan
type: plan
status: processed
tags: [trading-system, milestone-6, market-data, providers, yfinance, massive]
created: 2026-04-27
processed: 2026-04-27
---

# Milestone 6B Provider Boundary Hardening Plan

## Summary

Milestone 6B was planned as one implementation issue:

```text
Milestone 6B Issue 1 - Harden market data provider boundary
```

The goal was not to add Massive.com yet. The goal was to stop wiring the current yfinance implementation directly through the CLI so the next provider can be added cleanly in Milestone 6C.

The problem at plan time:

- `fetch-market-data` directly constructed `YFinanceDailyOHLCVImportSource`
- the CLI hardcoded `source="yfinance"`
- the CLI passed yfinance-specific `source_ref`

That was acceptable for Milestone 6A, but Milestone 6C needs a provider boundary that can support another provider.

## Planned Key Changes

- Add an application-level provider selection boundary for read-only daily OHLCV fetches.
- Keep yfinance as the only implemented provider in 6B.
- Add explicit CLI support:

```powershell
uv run trading-system fetch-market-data AAPL --provider yfinance --start 2026-04-01 --end 2026-04-30
```

- Preserve backward compatibility:

```powershell
uv run trading-system fetch-market-data AAPL --start 2026-04-01 --end 2026-04-30
```

should still use yfinance.

- Move provider selection out of direct CLI construction and into a small provider registry or factory.
- Preserve existing snapshot behavior:
  - context type remains `daily_ohlcv`
  - stored source remains `yfinance`
  - payload shape remains unchanged
  - `MarketContextSnapshot` remains the storage boundary
  - existing target and instrument validation remains in `MarketContextImportService`

## Non-Scope

Milestone 6B should not add:

- Massive.com integration
- API keys
- provider config files
- live data
- intraday data
- options chains
- news or fundamentals
- automatic refresh
- provider fallback behavior
- provider recommendations
- thesis verification
- AI or ML interpretation
- domain entity changes
- snapshot schema migration

## Planned Test Coverage

Focused tests should cover:

- default `fetch-market-data` still uses yfinance
- explicit `--provider yfinance` stores the same `daily_ohlcv` snapshot shape
- unsupported provider selection fails clearly
- registry/factory returns yfinance source metadata
- existing linking behavior still works for trade-plan targets
- existing requirement still holds: unlinked fetch requires `--instrument-id`

Planned validation commands:

```powershell
uv run pytest tests\test_yfinance_market_data_source.py tests\test_cli_market_data_fetch.py
uv run pytest
```

## Implementation Outcome

Milestone 6B Issue 1 was implemented after this plan.

Implemented outcomes:

- added `src/trading_system/infrastructure/market_data_providers.py`
- added provider registry and source-selection metadata
- updated `fetch-market-data` to accept optional `--provider yfinance`
- preserved default yfinance behavior when `--provider` is omitted
- added unsupported-provider failure behavior
- added focused registry and CLI tests
- updated app repo README, STATUS, roadmap, Milestone 6 design, and closeout docs
- updated knowledge-base Milestone 6/status/index pages

Recorded validation:

```text
10 focused provider-boundary tests passed
166 full-suite tests passed
```

The authoritative implementation closeout is:

```text
C:\Users\bosto\dockerstuff\trading-system\DOCS\milestone-6b-provider-boundary-hardening-closeout.md
```

## Related Pages

- [[milestone-6-market-data-provider-boundary]]
- [[application-implementation-status]]
- [[product-roadmap-and-learning-boundaries]]
