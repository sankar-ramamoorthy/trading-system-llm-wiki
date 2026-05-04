---
title: Milestone 15 Alpaca Read-Only Market Data Provider
type: topic
status: complete
tags: [trading-system, milestone-15, alpaca, market-data, options-chain, market-context]
created: 2026-05-04
updated: 2026-05-04
---

# Milestone 15 Alpaca Read-Only Market Data Provider

Milestone 15 is complete in the linked application repo.

The milestone adds Alpaca as a read-only provider behind the existing market-context boundary:

- `fetch-market-data --provider alpaca`
- `fetch-options-chain --provider alpaca`
- vault-first, environment-fallback credentials for `ALPACA_API_KEY` and `ALPACA_SECRET_KEY`
- daily OHLCV snapshots through Alpaca stock bars with the free-tier IEX feed
- options chain snapshots through Alpaca option snapshots with the free-tier indicative feed
- all output stored only as `MarketContextSnapshot`

## Boundary

Alpaca market data remains separate from Alpaca broker execution. The provider adapters do not submit orders, sync broker status, reconcile broker orders, create recommendations, interpret context, mutate trades, add live streaming, schedule refreshes, or fall back automatically to another provider.

Provider response objects stay inside infrastructure adapters. The application stores normalized, advisory context snapshots.

## Validation

Recorded in the application repo on 2026-05-04:

- `uv run pytest tests\test_market_data_provider_registry.py tests\test_cli_market_data_fetch.py tests\test_alpaca_market_data_source.py tests\test_alpaca_options_chain_source.py`: 27 passed
- `uv run pytest`: 305 passed

## Source Documents

- [[implemented-milestone-15-alpaca-read-only-market-data-provider-20260504]]
- Application repo `DOCS/milestone-15-issue-map.md`
- Application repo `DOCS/product-roadmap.md`
- Application repo `STATUS.md`
- [[post-milestone-11-roadmap]]
- [[product-roadmap-and-learning-boundaries]]
