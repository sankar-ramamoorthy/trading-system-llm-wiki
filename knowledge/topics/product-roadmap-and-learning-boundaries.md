---
title: Product Roadmap And Learning Boundaries
type: topic
status: active
tags: [trading-system, roadmap, learning-systems, reinforcement-learning, product-boundaries]
created: 2026-04-26
updated: 2026-04-26
---

# Product Roadmap And Learning Boundaries

The product roadmap now has two tracks:

- the application repository carries the implementation-facing roadmap
- this wiki page carries the synthesized product memory and reasoning

The current roadmap should be read as near-term accepted milestones plus longer-term product direction. The long-term direction does not replace the accepted Milestones 4 and 5 scope.

## Current Near-Term Roadmap

The accepted near-term sequence remains:

1. Milestone 4: read-only market context
2. Milestone 5: review, learning, and local operations

Milestone 3 and Milestone 4 are complete. Milestone 5 has started.

Milestone 4 added read-only market and context support while preserving the system as the canonical owner of trade meaning.

Milestone 5 should improve review structure, narrow journal-grade reporting, export, and local operations. Its first implemented slices are creation-time review tags/filtering and optional review quality scores. It should not expand into AI-generated review content or reinforcement learning.

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

The ADR records the durable decision: AI, ML, and RL are deferred beyond the accepted Milestones 3 through 5 roadmap.

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

## Processing Notes

The raw brainstorm predates current project state. It should not supersede the current milestone pages.

The promoted durable idea is the boundary, not the full V1-V7 sequence. The V1-V7 sequence remains long-term product direction until a later roadmap update accepts it.
