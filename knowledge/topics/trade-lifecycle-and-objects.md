---
title: Trade Lifecycle and Objects
type: topic
status: active
tags: [trading-system, trade-lifecycle, domain-model]
created: 2026-04-19
updated: 2026-04-26
---

# Trade Lifecycle and Objects

The system should model trading as a lifecycle, not a single transaction record.

## Structured Trade Object

Every trade candidate should have:

- playbook
- timeframe
- core thesis
- why now
- invalidation
- setup notes
- risk plan
- monitoring checklist
- expected price behavior
- expected context behavior
- acceptable and unacceptable adverse developments

## Lifecycle Stages

Idea creation:

- created from manual observation, scanner, TradingView alert, or research
- captured as a `TradeIdea` with instrument, playbook, purpose, direction, horizon, and status

Thesis and planning:

- `TradeThesis` records why the trade exists, including reasoning, evidence, risks, and disconfirming signals
- `TradePlan` records how the trade will be executed, including entry criteria, invalidation, targets, risk model, and sizing assumptions
- thesis and plan revisions remain traceable over time

Pre-trade discipline:

- rules engine checks risk, size, event proximity, exposure overlap, and playbook constraints
- context engine checks filings, news, competitors, sector tone, market regime, chart state, and setup freshness

Active monitoring:

- hard state tracks P&L, size, time in trade, exposure, stops, and planned management steps
- context state tracks company events, peer read-through, regime shifts, chart changes, thesis decay, and setup quality

Execution:

- `OrderIntent` records intended execution separately from actual fills
- `Fill` records execution reality separately from trade intent and position meaning
- current code records fills manually against open positions and can link them to an `OrderIntent`
- position execution state tracks total bought quantity, total sold quantity, current open quantity, and weighted average entry price
- a reducing fill to zero closes the position with close reason `fills_completed`

Review:

- journals and reviews evaluate whether the outcome followed the plan
- thesis revisions should remain traceable to avoid rewriting history
- Milestone 1 allows one manual immutable `TradeReview` per closed position
- Milestone 5 adds creation-time review tags for categorizing and filtering completed reviews
- Milestone 5 adds optional creation-time process, setup, execution, and exit quality scores

## First Executable Flow

The first implemented flow is complete:

```text
TradeIdea -> TradeThesis -> TradePlan -> Rule Checks -> Decision -> Position -> Fill -> Position close -> TradeReview
```

This is represented by the canonical Milestone 1 demo before adding watchlists, AI context, broker integration, order-intent modeling, P&L, or broader market data.

The current post-MVP code path now inserts `OrderIntent` before manual fills:

```text
TradeIdea -> TradeThesis -> TradePlan -> plan approval -> RuleEvaluation -> OrderIntent -> Position -> Fill -> Position close -> TradeReview
```

## Important Separations

Keep `TradeIdea`, `TradeThesis`, `TradePlan`, `Position`, `OrderIntent`, `BrokerOrder`, `Fill`, `JournalEntry`, and `TradeReview` separate.

In the v2 domain model, a `Position` must originate from a `TradePlan`. It should not be created directly from a `TradeIdea`; any idea link on the position is derived through the plan.

Review tags and quality scores are metadata on `TradeReview`, not separate taxonomy, analytics, or coaching entities in the current implementation.

## Related Pages

- [[canonical-domain-model]]
- [[context-intelligence-layer]]
- [[first-vertical-slice]]
- [[mvp-definition-and-boundaries]]
- [[milestone-5-review-tags-and-filtering]]
- [[milestone-5-review-quality-scores]]
