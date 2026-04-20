---
title: First Vertical Slice
type: topic
status: active
tags: [trading-system, vertical-slice, implementation]
created: 2026-04-19
updated: 2026-04-20
---

# First Vertical Slice

The first vertical slice is complete as the Milestone 1 MVP. It proves that the system can carry one planned discretionary swing trade from idea through review without external integrations.

## Current Implementation Status

As of the 2026-04-20 Milestone 1 summary, ADR-005, and updated app README, the implemented local workflow is:

```text
TradeIdea -> TradeThesis -> TradePlan -> plan approval -> RuleEvaluation -> Position -> Fill -> Position close -> TradeReview
```

Implemented pieces include the Python scaffold, planned trade workflow skeleton, deterministic rule evaluation for an approved plan, opening a position from an approved plan, manual fill recording, execution-state tracking, automatic close from fills, one manual review for a closed position, and lifecycle events.

The canonical demo command is:

```powershell
uv run trading-system demo-planned-trade
```

## Scope

Milestone 1 built the planned discretionary swing trade path:

1. Create `TradeIdea`
2. Attach `TradeThesis`
3. Create `TradePlan`
4. Manually approve the plan
5. Run deterministic rule checks before the trade decision
6. Enter a recorded position linked to the approved plan
7. Record manual entry and exit fills
8. Track position state
9. Close the position from fills
10. Write one manual `TradeReview`

The reference example is a swing long in NVDA on a pullback to the 20-day moving average, with invalidation below a prior low.

## Explicit Deferrals

These were intentionally excluded from the first slice:

- watchlists
- AI or context ingestion
- broker integration
- market data ingestion
- options contracts
- order-intent modeling
- reconciliation subsystem
- regime assessment
- P&L and analytics
- dashboards or web UI
- force-close, reopen, or fill-correction workflows

These are important later, but they will distract from proving the core trade lifecycle.

## Required Entity Subset

The first slice should use only the minimum serious subset:

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

This subset is narrower than the full [[canonical-domain-model]].

## First Deterministic Rules

Start with three to five explicit, machine-evaluable rules:

- maximum risk per trade, such as no more than 1% of account
- plan approval requires invalidation
- position must originate from an approved plan
- actual risk must stay within plan tolerance
- no duplicate active positions per instrument, if useful early

Each rule should be stored as `Rule` metadata, evaluated into `RuleEvaluation`, and able to create a `Violation`.

## Interface

The first interface is CLI-driven. The canonical demo exercises the lifecycle in one command; future Milestone 2 CLI work can split the lifecycle into practical commands:

- `create_idea`
- `add_thesis`
- `create_plan`
- `approve_plan`
- `run_rule_checks`
- `open_position`
- `record_fill`
- `write_review`

Position close is not a separate MVP command. It is a domain transition caused by reducing fills that bring open quantity to zero.

## Data Direction

Use Postgres early enough to make the model real. Use UUIDs, `created_at`, `updated_at`, explicit foreign keys, and JSONB for flexible plan or thesis details where appropriate.

The first persistent schema should cover instruments, trade ideas, theses, plans, positions, manual fills, rules, rule evaluations, violations, reviews, and lifecycle events. Milestone 1 uses in-memory repositories for the demo while SQLAlchemy infrastructure remains scaffolded.

## Lifecycle Events

Every major action should emit a lifecycle event:

- `PLAN_APPROVED`
- `POSITION_OPENED`
- `FILL_RECORDED`
- `RULE_VIOLATION_DETECTED`
- `POSITION_CLOSED`
- `TRADE_REVIEW_CREATED`

This event stream is the audit trail and the future substrate for analysis.

## Success Criterion

The Milestone 1 success criterion is met: one fully tracked planned trade can move from intent through manual execution, close, and review in the local demo.

Next work should follow [[milestone-2-roadmap]] rather than expanding Milestone 1 scope retroactively.

## Related Pages

- [[trade-lifecycle-and-objects]]
- [[deterministic-rules-vs-contextual-intelligence]]
- [[application-project-structure]]
- [[application-implementation-status]]
- [[mvp-definition-and-boundaries]]
- [[milestone-2-roadmap]]
