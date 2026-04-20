---
title: Data and Platform Strategy
type: topic
status: active
tags: [trading-system, data, platforms]
created: 2026-04-19
updated: 2026-04-19
---

# Data and Platform Strategy

The system should remain independent and integrate outward through adapters. No external platform owns the system logic or canonical data model.

## External Platforms

TradingView is useful for charting, discretionary scanning, alerts, webhooks, and visual validation. It should not be the system brain.

thinkorswim and Schwab are useful for options analysis, discretionary execution, and possible future broker integration. They should not define the architecture.

Alpaca is a likely first programmable execution venue, especially for paper trading and initial broker integration.

## Data Providers

Start with free or low-cost data and upgrade only when limits become real.

Initial and candidate sources:

- Yahoo Finance
- Alpha Vantage
- Polygon.io as a future upgrade

Data quality, alignment, and structure matter more than cost early.

## Adapter Principle

External systems provide facts:

- brokers provide orders, fills, balances, and raw positions
- market data providers provide quotes, bars, options chains, and metadata
- context providers provide filings, news, peer events, macro events, and policy events

The trading system interprets meaning.

## Related Pages

- [[canonical-domain-model]]
- [[architecture-overview]]
