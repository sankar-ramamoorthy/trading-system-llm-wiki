---
title: Canonical Domain Model
type: entity
status: active
tags: [trading-system, domain-model]
created: 2026-04-19
updated: 2026-04-26
---

# Canonical Domain Model

The domain model separates trading concepts by meaning, timing, and ownership. Do not collapse idea, thesis, plan, position, order, fill, and review into one object.

## Source-of-Truth Boundary

The system owns meaning. External systems provide facts.

The system is source of truth for trade ideas, playbooks, thesis, plans, position purpose, lifecycle state, rules, rule evaluations, violations, journals, and reviews.

Brokers are source of truth for orders, fills, balances, and raw broker-reported positions. Market data providers are source of truth for bars, quotes, option chains, and contract metadata. Context sources are source of truth for filings, news, peer events, and macro or policy events, but not interpretation.

## Entity Groups

Market identity:

- `Instrument`
- `OptionContract`
- `Universe`
- `UniverseMembership`

Opportunity and planning:

- `Playbook`
- `TradeIdea`
- `TradeThesis`
- `TradePlan`
- `WatchlistItem`

Position and execution:

- `Position`
- `PositionLot`
- `OrderIntent`
- `BrokerOrder`
- `Fill`
- `BrokerAccount`

Rules and discipline:

- `Rule`
- `RuleEvaluation`
- `Violation`

Context and monitoring:

- `ContextEvent`
- `ContextLink`
- `ThesisAssessment`
- `RegimeAssessment`

Review and lifecycle:

- `JournalEntry`
- `TradeReview`
- `RevisionLog`
- `LifecycleEvent`

Reconciliation:

- `ExternalMapping`
- `ReconciliationRun`
- `ReconciliationIssue`

## Core Invariants

- No `Position` should exist without meaning.
- A `Position` originates from a `TradePlan`, not directly from a `TradeIdea`.
- `linked_trade_idea` on a position is derived through the plan relationship.
- `TradeThesis` and `TradePlan` are separate objects: thesis explains why; plan defines how.
- External facts do not override internal meaning.
- Meaning-bearing objects require revision or lifecycle history.
- Context can advise, but it does not bypass deterministic rules.

## V1 Core

The application repo `DOCS/domain-model.md` v2 and ADR-005 define the narrow Milestone 1 MVP implementation scope:

- `Instrument`
- `TradeIdea`
- `TradeThesis`
- `TradePlan`
- `Position`
- `Fill` using manual entry
- `Rule`
- `RuleEvaluation`
- `Violation`
- `TradeReview`
- `LifecycleEvent`

This is intentionally smaller than the full canonical domain model. It proves the core lifecycle before watchlists, broker orders, `OrderIntent`, context ingestion, reconciliation, P&L, analytics, and broader market identity features are implemented.

Post-Milestone-1 implementation has now introduced a narrow `OrderIntent` object in the application repo. It remains intentionally smaller than broker-order modeling and should still be understood as system intent, not external execution fact.

## First Slice Subset

The first executable slice currently matches the narrow v1 implementation scope and is complete as Milestone 1. It proves a planned discretionary swing trade from idea through review before watchlists, context ingestion, broker integration, and reconciliation are added.

Required first-slice entities:

- `Instrument`
- `TradeIdea`
- `TradeThesis`
- `TradePlan`
- `Position`
- `Fill`
- `Rule`
- `RuleEvaluation`
- `Violation`
- `TradeReview`
- `LifecycleEvent`

Deferred until after the first slice:

- `OptionContract`
- `Universe`
- `WatchlistItem`
- `OrderIntent`
- `BrokerOrder`
- `BrokerAccount`
- `ContextEvent`
- `ThesisAssessment`
- `RegimeAssessment`
- `JournalEntry`
- `RevisionLog`
- `ExternalMapping`
- `ReconciliationRun`
- `ReconciliationIssue`

Current post-slice additions observed in code:

- `OrderIntent` is now implemented narrowly between approved `TradePlan` and manual `Fill`
- realized P&L exists as a read-side calculation for closed positions, not as a persisted canonical entity
- `MarketContextSnapshot` is implemented for read-only, non-canonical context snapshots
- `TradeReview.tags` is implemented as creation-time review metadata for filtering and learning loops

## Related Pages

- [[trade-lifecycle-and-objects]]
- [[deterministic-rules-vs-contextual-intelligence]]
- [[first-vertical-slice]]
- [[mvp-definition-and-boundaries]]
- [[milestone-5-review-tags-and-filtering]]
