---
title: Context Intelligence Layer
type: topic
status: active
tags: [trading-system, context-intelligence, ai]
created: 2026-04-19
updated: 2026-04-19
---

# Context Intelligence Layer

The context intelligence layer answers what changed, how relevant it is, and whether the world still supports a trade or watchlist setup.

## Context Ingestion

`context_ingestion` gathers and normalizes filings, corporate disclosures, earnings updates, news, macro events, peer developments, sector behavior, price structure, options behavior, and later breadth or regime proxies.

Internal event types may include `FilingEvent`, `NewsEvent`, `PeerEvent`, `MacroEvent`, `PriceStructureEvent`, `OptionsFlowObservation`, and `RegimeObservation`.

## Context Store

`context_store` should support comparison across time:

- what changed since entry?
- what changed since watchlist creation?
- what changed since last review?

Each item should carry timestamp, source, related symbols, event type, raw reference, normalized summary, confidence or relevance score, and affected ideas, watchlist items, or positions.

## Thesis Monitor

`thesis_monitor` compares new facts to the original thesis, supporting facts, risks, disconfirming triggers, and monitoring checklist.

Outputs should include `thesis_unchanged`, `thesis_strengthened`, `thesis_weakened`, `thesis_invalidated`, and `review_required`.

## Regime Detection

Regime detection should classify market-wide, sector-level, and symbol-level conditions. Labels are useful only when they change behavior.

Useful labels include `trending_up`, `trending_down`, `range_bound`, `choppy_high_vol`, `orderly_pullback`, `unstable_event_driven`, `breakout_friendly`, and `mean_reversion_friendly`.

Every regime assessment should map to playbook implications.

## Watchlist Monitor

`watchlist_monitor` tracks names before they become trades. It should identify whether a setup is improving, degrading, approaching a catalyst, no longer qualified, or ready for review.

Example states: `candidate`, `forming`, `ready_for_review`, `degrading`, `invalid`, `archived`, and `high_priority`.

## Related Pages

- [[deterministic-rules-vs-contextual-intelligence]]
- [[trade-lifecycle-and-objects]]
