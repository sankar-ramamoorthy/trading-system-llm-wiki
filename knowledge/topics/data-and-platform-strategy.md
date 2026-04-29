---
title: Data and Platform Strategy
type: topic
status: active
tags: [trading-system, data, platforms]
created: 2026-04-19
updated: 2026-04-29
---

# Data and Platform Strategy

The system should remain independent and integrate outward through adapters. No external platform owns the system logic or canonical data model.

## External Platforms

TradingView is useful for charting, discretionary scanning, alerts, webhooks, and visual validation. It should not be the system brain.

thinkorswim and Schwab are useful for options analysis, discretionary execution, and possible future broker integration. They should not define the architecture.

Alpaca is a likely first programmable execution venue, especially for paper trading and initial broker integration.

## Data Providers

Market data has moved into Milestone 6 as read-only provider integration.

ADR-007 accepts optional prototype-grade `yfinance` as the first provider stance and daily OHLCV history as the first data shape. ADR-009 accepts Massive.com as the next provider candidate. Provider output must be stored as `MarketContextSnapshot` records before the rest of the application uses it.

Market data remains advisory context rather than a driver of automated decisions. Start with free or low-cost data and upgrade only when limits become real.

Initial and candidate sources:

- Yahoo Finance
- Alpha Vantage
- Massive.com, formerly Polygon.io

Data quality, alignment, and structure matter more than cost early.

Provider credentials are an operational boundary. Massive.com currently uses `MASSIVE_API_KEY`; keys should stay in the local environment or another explicit local configuration mechanism and must not be committed, logged, or stored inside snapshots.

Broker integration, execution-grade quotes, live streaming, and provider-driven recommendations remain later concerns.

## Adapter Principle

External systems provide facts:

- brokers provide orders, fills, balances, and raw positions
- market data providers provide quotes, bars, options chains, and metadata
- context providers provide filings, news, peer events, macro events, and policy events

The trading system interprets meaning.

## Related Pages

- [[canonical-domain-model]]
- [[architecture-overview]]
- [[milestone-6-market-data-provider-boundary]]
