---
title: Milestone 2 Roadmap
type: topic
status: active
tags: [trading-system, milestone-2, roadmap]
created: 2026-04-20
updated: 2026-04-20
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

## Candidate Work Areas

Durable persistence:

- persist `TradeIdea`, `TradeThesis`, `TradePlan`, `Position`, `Fill`, `TradeReview`, and `LifecycleEvent`
- add Alembic migrations and repository implementations
- keep repository interfaces stable where possible

Query and retrieval workflows:

- list positions
- show position
- list closed positions
- show associated fills and review

`OrderIntent`:

- introduce planned execution intent between `TradePlan` and actual `Fill`
- preserve the distinction between what the trader intended to execute and what actually filled
- keep broker integration out of scope for this step

Basic P&L:

- compute minimal realized P&L for simple closed positions from fills
- avoid tax lots, commissions, fees, and advanced reporting initially

Lifecycle timeline:

- expose ordered lifecycle events for a position
- possible command: `uv run trading-system show-position-timeline <position-id>`

CLI usability:

- move beyond demo-only usage
- add simple commands for creating ideas, theses, plans, approvals, rule checks, positions, fills, and reviews

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
