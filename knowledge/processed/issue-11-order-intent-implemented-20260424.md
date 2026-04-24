---
title: Issue 11 - Narrow OrderIntent Implementation
type: note
status: draft
tags: [trading-system, issue-11, order-intent, implementation]
created: 2026-04-24
updated: 2026-04-24
---

# Issue 11 - Narrow OrderIntent Implementation

This note synthesizes the raw Issue 11 planning and implementation notes against the current application repository state in `C:\Users\bosto\dockerstuff\trading-system`.

## Summary

Issue 11 was implemented as a narrow introduction of `OrderIntent` into the executable trade workflow.

The implemented approach inserts `OrderIntent` between approved `TradePlan` and manual `Fill`, while keeping current `Position` creation timing unchanged.

Implemented intent:

- add a first-class `OrderIntent` domain object
- persist `OrderIntent` in in-memory and JSON repositories
- require approved plan plus persisted passing `RuleEvaluation` records before creating an `OrderIntent`
- allow `Fill` to optionally reference `order_intent_id`
- surface linked order intents in retrieval output
- update the demo to create an order intent before recording linked fills

Explicitly not implemented in Issue 11:

- broker order modeling
- broker integration
- moving `Position` creation to first fill
- richer `OrderIntent` status transitions beyond initial creation

## Implemented Objects And Interfaces

Observed in the application repo:

- new domain entity: `src/trading_system/domain/trading/order_intent.py`
- new enums: `OrderSide`, `OrderType`, `OrderIntentStatus`
- `Fill` now includes optional `order_intent_id: UUID | None`
- `OrderIntentRepository` was added to repository ports
- `RuleEvaluationRepository` now supports `list_by_entity(entity_type, entity_id)`
- new service: `CreateOrderIntentService`

The `OrderIntent` model is intentionally narrow and local to system intent:

- `id`
- `trade_plan_id`
- `symbol`
- `side`
- `order_type`
- `quantity`
- optional `limit_price`
- optional `stop_price`
- `status`
- `created_at`
- optional `notes`

## Workflow Behavior After Issue 11

The implemented executable flow in code is now:

```text
TradeIdea -> TradeThesis -> TradePlan -> plan approval -> RuleEvaluation -> OrderIntent -> Position -> Fill -> Position close -> TradeReview
```

Important detail:

- `Position` is still opened by `PositionService.open_position_from_plan(...)`
- `OrderIntent` is added before fills, but it does not yet control when a position begins to exist
- fills remain the facts of execution reality
- position state derivation from fills remains unchanged

## Rule-Gating Behavior

`CreateOrderIntentService` enforces four preconditions:

- the trade plan must exist
- the trade plan must be approved
- at least one persisted rule evaluation must exist for that plan
- all persisted rule evaluations for that plan must have passed

This means Issue 11 treats `OrderIntent` as a post-discipline object, not just a lightweight note.

## Fill Linkage Behavior

`FillService.record_manual_fill(...)` now accepts optional `order_intent_id`.

When provided, the service:

- loads the order intent
- rejects missing order intents
- verifies the order intent belongs to the same `trade_plan_id` as the position
- persists the fill with that linkage

When omitted, legacy manual-fill behavior still works.

This preserves backward compatibility for older persisted data and for workflows where fills are recorded manually without a prior intent object.

## Retrieval And Demo Changes

Issue 11 also changed the read side:

- JSON persistence now includes an `order_intents` collection
- JSON serialization supports `OrderIntent`
- JSON fill serialization supports optional `order_intent_id`
- `PositionQueryService.PositionDetail` now includes `order_intents`
- `show-position` prints linked order intents before fills
- `demo-planned-trade` now creates an order intent before opening a position and recording linked fills

`show-position-timeline` remains focused on `Position` lifecycle events.

`OrderIntent` lifecycle events exist separately and are not yet surfaced by a dedicated CLI retrieval path.

## Validation Observed

The implementation note recorded:

```text
uv run pytest -> 53 passed in 3.11s
```

This is consistent with the application repo session that implemented Issue 11.

## Documentation Drift To Keep In Mind

There is currently a small but important documentation mismatch between code and some application repo documents.

Current code behavior includes `OrderIntent` in the executable workflow.

However, some application repo documents still describe the older flow:

```text
TradeIdea -> TradeThesis -> TradePlan -> plan approval -> RuleEvaluation -> Position -> Fill -> Position close -> TradeReview
```

Examples observed during this synthesis:

- `README.md`
- `DOCS/ADR/005-mvp-definition-and-boundaries.md`
- `DOCS/milestone-1-summary.md`

This knowledge-base note records the newer implemented state without silently rewriting those application repo documents.

## Interpretation

Issue 11 materially improves separation between planning intent and execution facts while staying conservative about lifecycle refactoring.

That is a good fit for the project rules because it:

- preserves auditability
- keeps `OrderIntent` distinct from `Fill`
- avoids prematurely collapsing `OrderIntent` into broker-order semantics
- avoids a larger `Position` timing refactor in the same issue

## Related Notes

- [[application-implementation-status]]
- [[trade-lifecycle-and-objects]]
- [[mvp-definition-and-boundaries]]
- [[milestone-2-roadmap]]
