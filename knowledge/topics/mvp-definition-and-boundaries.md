---
title: MVP Definition and Boundaries
type: topic
status: active
tags: [trading-system, mvp, milestone-1, boundaries]
created: 2026-04-20
updated: 2026-04-20
---

# MVP Definition and Boundaries

ADR-005 defines the Milestone 1 MVP as a local, CLI-driven trading system that enforces structured trade intent, captures manual execution, closes positions from fills, and supports manual post-trade review with auditability.

## MVP Workflow

```text
TradeIdea -> TradeThesis -> TradePlan -> plan approval -> RuleEvaluation -> Position -> Fill -> Position close -> TradeReview
```

`LifecycleEvent` records auditable transitions throughout the workflow.

## Included

The MVP includes:

- `TradeIdea`, `TradeThesis`, and `TradePlan`
- plan approval state
- deterministic rule evaluation
- `RuleEvaluation` and `Violation`
- `Position` opened from an approved `TradePlan`
- manual `Fill` recording
- position execution-state tracking
- automatic position close when fills reduce open quantity to zero
- one manual `TradeReview` per closed position
- lifecycle events for audit history
- CLI demo using in-memory repositories

## Excluded

The MVP explicitly excludes:

- broker integration
- automated order placement
- order synchronization
- market data ingestion
- AI or ML features
- news, filings, or context ingestion
- reconciliation workflows
- P&L calculations
- commissions, fees, and slippage modeling
- performance analytics, dashboards, and reports
- REST API or web UI
- `OrderIntent`
- multi-leg trades
- reversal workflows
- fill correction or amendment workflows
- force-close or reopen workflows
- review editing, versioning, or multiple reviews per position

## Principles

The MVP prioritizes:

- discipline over automation
- explicit structure over convenience
- auditability over optimization
- correctness over completeness
- domain clarity over integration breadth

Broker integration, market data, analytics, and AI remain later concerns because the system must first represent what was planned, what happened, when it closed, and what was learned.

## Related Pages

- [[first-vertical-slice]]
- [[application-implementation-status]]
- [[milestone-2-roadmap]]
- [[canonical-domain-model]]
