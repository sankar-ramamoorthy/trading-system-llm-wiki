---
title: Product Roadmap And Learning Boundaries
type: topic
status: active
tags: [trading-system, roadmap, learning-systems, reinforcement-learning, product-boundaries]
created: 2026-04-26
updated: 2026-05-02
---

# Product Roadmap And Learning Boundaries

The product roadmap now has two tracks:

- the application repository carries the implementation-facing roadmap
- this wiki page carries the synthesized product memory and reasoning

The current roadmap should be read as near-term accepted milestones plus longer-term product direction. The long-term direction does not replace the accepted Milestone 6 provider boundary.

## Current Near-Term Roadmap

The accepted near-term sequence has advanced through:

1. Milestone 4: read-only market context
2. Milestone 5: review, learning, and local operations
3. Milestone 6: read-only market data provider integration
4. Milestone 7: API-first trade capture workspace
5. Milestone 8: options chain ingestion

Milestones 3 through 8 are complete. Milestone 9 Web Product Beyond First Capture is the next planned slice.

Milestone 4 added read-only market and context support while preserving the system as the canonical owner of trade meaning.

Milestone 5 improved review structure, narrow journal-grade reporting/export, and local operations. Its implemented slices are creation-time review tags/filtering, optional review quality scores, Markdown journal export, and local JSON operations.

Milestone 6 adds read-only daily OHLCV provider integration behind the `MarketContextSnapshot` boundary. The first accepted provider stance is optional prototype-grade `yfinance`; Massive.com is now the first credentialed provider. Milestone 6 should not expand into live streaming, execution-grade quotes, broker integration, provider-driven recommendations, or AI-generated interpretation.

The first Milestone 6 slice, Milestone 6A, is complete. It is implemented as `fetch-market-data`, which stores daily OHLCV snapshots as explicit local market context. Validation on 2026-04-27 recorded 6 focused yfinance tests passing and 162 full-suite tests passing.

Milestone 6B Issue 1 is also complete. It adds explicit provider selection through `--provider yfinance`, keeps yfinance as the default provider, and moves provider-backed source selection behind a small registry boundary. Validation on 2026-04-27 recorded 10 focused provider-boundary tests passing and 166 full-suite tests passing.

Milestone 6C Issue 1 is complete. ADR-009 accepts Massive.com, formerly Polygon.io, as the next provider candidate after yfinance, with the official `massive` Python client as the preferred first implementation path, `MASSIVE_API_KEY` as the credential boundary, and daily aggregate/OHLCV-style bars as the first data shape.

Milestone 6C Issue 2 is complete. The linked application repo now supports `fetch-market-data --provider massive`, requires `MASSIVE_API_KEY`, normalizes Massive daily aggregate bars into `daily_ohlcv` snapshots, and keeps yfinance as the default provider. Validation on 2026-04-29 recorded 21 focused market-data tests passing and 177 full-suite tests passing.

Milestone 6D is complete. Milestone 6 is closed with yfinance and Massive.com behind the provider boundary.

ADR-008 has now been implemented through Milestone 7 as the local API-first trade capture workspace. Milestone 8 added options chain ingestion as the first market data depth extension.

Milestone 9 should deepen the browser product without crossing into execution. The current proposed plan is a plan-centered local workbench: saved plan list/detail views, browser approval for draft plans, and metadata-only context attachment by copying existing snapshots to plans.

Milestone 10 is the current secure-credentials candidate. It should not be pulled into Milestone 9 web product depth.

## External Product Assessment Notes

A Perplexity assessment based only on the application repo `README.md` and `STATUS.md` reinforced the current product direction rather than changing it.

Durable takeaways:

- The system's strongest product identity is discipline, auditability, and explicit intent/execution separation.
- The biggest execution risk is overbuilding before the daily workflow is fast enough for consistent use.
- Market context remains valuable only if it stays evidence, not authority or quasi-decision logic.
- Review quality depends on meaningful prompts and labels, not just more review fields.
- Lifecycle transitions and invariants should stay heavily tested because they are the trust boundary of the tool.

Near-term planning should treat these as guardrails, not new scope.

## Long-Term Product Direction

The shared brainstorm introduced a longer product direction:

```text
V1 - Trading workflow foundation
V2 - Simulator / scenario replay
V3 - Insight engine and reporting
V4 - AI-assisted pattern explanation
V5 - RL / policy simulation
V6 - Paper trading integration
V7 - Real-money readiness gate
```

This is useful product thinking, but it is not yet an accepted implementation sequence. It should be reconciled after the near-term Milestone 5 learning and local-operations work is better established.

The most important long-term idea is that the system may become a training, simulation, review, and decision-support system before the user returns to real-money trading.

## Learning-System Boundary

The core rule is:

```text
No intelligence before truth.
```

AI and RL should wait until the system has stable workflows, consistent reviews, reliable labels, enough completed trades or scenarios, and explicit success/failure definitions.

The current application should first generate trustworthy ground truth:

- structured trade intent
- approved plans
- rule evaluations
- fills and position lifecycle
- review tags and lessons
- review quality scores
- completed outcomes

Future learning systems can use that data only after the data is mature enough to support meaningful conclusions.

## ADR Relationship

The companion ADR is:

- `C:\Users\bosto\dockerstuff\trading-system\DOCS\ADR\006-deferred-learning-systems-boundary.md`
- `C:\Users\bosto\dockerstuff\trading-system\DOCS\ADR\007-market-data-provider-boundary.md`
- `C:\Users\bosto\dockerstuff\trading-system\DOCS\ADR\009-massive-provider-boundary.md`

ADR-006 records the durable decision that AI, ML, and RL are deferred beyond the accepted near-term roadmap. ADR-007 records the durable market data provider boundary for Milestone 6. ADR-009 records the Massive.com provider boundary.

The roadmap page records evolving sequence:

- `C:\Users\bosto\dockerstuff\trading-system\DOCS\product-roadmap.md`

This wiki page preserves the reasoning, source brainstorm, and reconciliation with current project state.

## Source Notes

This page synthesizes:

- [[milestones-3-to-5-roadmap]]
- [[milestone-3-closeout]]
- [[deterministic-rules-vs-contextual-intelligence]]
- [[mvp-definition-and-boundaries]]
- [[brainstorm-20260426-product-roadmap-rl-boundaries]]
- `knowledge/processed/Analysis by Perplexity based on just the Repo README.md and Repo STATUS.md 2026-04-26 2_42 pm EST.md`
- [[milestone-6c-massive-daily-bars-implemented-20260429]]
- [[milestone-6-closeout-20260429]]
- [[milestone-9-web-product-beyond-first-capture]]

## Processing Notes

The raw brainstorm predates current project state. It should not supersede the current milestone pages.

The promoted durable idea is the boundary, not the full V1-V7 sequence. The V1-V7 sequence remains long-term product direction until a later roadmap update accepts it.
