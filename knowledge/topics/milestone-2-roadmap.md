---
title: Milestone 2 Roadmap
type: topic
status: active
tags: [trading-system, milestone-2, roadmap]
created: 2026-04-20
updated: 2026-04-24
---

# Milestone 2 Roadmap

Milestone 2 should make the completed Milestone 1 MVP more practical for real use without weakening the domain boundaries.

## Design Goal

Milestone 2 should focus on:

- durable persistence
- retrieving prior trades and positions
- clearer separation between intended execution and actual execution
- basic financial visibility
- CLI usability improvements

It should not expand into broker integration, market data ingestion, AI systems, dashboards, or broad analytics platforms.

## Observed Progress

Verified against the linked application repo on 2026-04-24:

- durable local JSON persistence is implemented
- read-only position retrieval and timeline commands are implemented
- narrow `OrderIntent` support is implemented and inserted before manual fills
- a minimal realized P&L calculation exists on the read side for closed positions
- explicit CLI write commands for the core workflow are implemented
- upstream read commands for trade ideas and trade plans are implemented

Milestone 2 is now largely implemented in code. The remaining work is best understood as final polish, cleanup, and documentation alignment rather than missing core capability.

## Candidate Work Areas

Durable persistence:

- implemented first as local JSON persistence in `.trading-system/store.json`
- persisted workflow now includes `TradeIdea`, `TradeThesis`, `TradePlan`, `Position`, `Fill`, `TradeReview`, `LifecycleEvent`, `RuleEvaluation`, `Violation`, and `OrderIntent`
- SQLite, Postgres, and migrations remain later options when stronger database behavior is justified
- repository interfaces should stay stable where possible

Query and retrieval workflows:

- implemented commands include `list-positions`, `show-position`, and `show-position-timeline`
- current position detail output includes linked fills, review, and order intents
- broader list and inspection workflows for upstream objects still remain useful follow-up work

`OrderIntent`:

- implemented narrowly as planned execution intent between approved `TradePlan` and manual `Fill`
- current code keeps `Position` creation timing unchanged
- fills can optionally link to an `OrderIntent`
- broker integration remains out of scope

Basic P&L:

- source code now computes minimal realized P&L for closed positions from fills on the read side
- commissions, fees, tax lots, portfolio aggregation, and advanced reporting remain deferred
- later 2026-04-24 application repo docs now reflect this implementation

Lifecycle timeline:

- implemented through `uv run trading-system show-position-timeline <position-id>`

CLI usability:

- practical write commands now exist beyond the demo workflow
- implemented read and write commands make the local JSON-backed workflow usable without opening the store file directly
- remaining CLI work is polish rather than missing core workflow coverage

## Follow-On Roadmap

The accepted roadmap after Milestone 2 is now recorded in [[milestones-3-to-5-roadmap]].

## Deferred

Unless a future ADR changes the boundary, Milestone 2 should still defer:

- broker integration
- automated execution
- real-time market data
- AI-generated insights
- dashboards or web UI
- portfolio-level analytics
- advanced reporting
- strategy automation

The final README update frames market data as read-only context and broker integration as a later adapter, not a source of trade meaning.

## Exit Criteria

Milestone 2 is complete when:

- core data persists across runs
- past positions and trades can be retrieved
- intended execution is distinct from actual fills
- basic P&L is available for simple closed trades
- CLI supports practical manual usage
- documentation reflects actual behavior

## Related Pages

- [[mvp-definition-and-boundaries]]
- [[application-project-structure]]
- [[canonical-domain-model]]
- [[development-workflow]]
