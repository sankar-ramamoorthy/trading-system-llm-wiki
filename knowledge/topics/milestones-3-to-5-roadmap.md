---
title: Milestones 3 To 5 Roadmap
type: topic
status: active
tags: [trading-system, roadmap, milestone-3, milestone-4, milestone-5]
created: 2026-04-24
updated: 2026-04-24
---

# Milestones 3 To 5 Roadmap

The accepted roadmap after Milestone 2 is:

1. Manual workflow usability
2. Read-only market context
3. Review, learning, and local operations

This sequence keeps the project local-first and manual-first while extending usefulness without weakening the domain boundaries.

## Current Status

- Milestone 1 is complete.
- Milestone 2 is complete.
- Milestone 3 is complete.
- Milestone 4 is next.

## Milestone 3

Milestone 3 should make the existing CLI workflow comfortable for repeated real use.

Focus areas:

- command ergonomics and output clarity
- smoother chaining from plan to execution to review
- practical manual workflow polish without changing the domain model

Observed progress from the linked application repo and new raw notes:

- review inspection commands now exist through `list-trade-reviews` and `show-trade-review`
- read-command output formatting has been normalized for consistent headers, empty states, section order, and optional-value formatting
- thesis inspection now exists through `list-trade-theses` and `show-trade-thesis`
- exact-match filtering and `oldest|newest` sorting now exist across the current list commands
- README examples have been updated so the documented CLI surface matches the current usability bundle
- `OrderIntent` cancellation now exists as a narrow follow-on with audit events and fill-linkage enforcement

The implemented work now supports an explicit Milestone 3 completion call.

Milestone 3 is complete because:

- routine manual workflows no longer depend on the demo command
- the CLI now supports practical creation, inspection, execution, cancellation, and review chaining
- usability improvements stayed inside the manual and auditable domain boundary
- the work did not spill into market context, reporting, or automation concerns that belong to later milestones

See [[milestone-3-closeout]] for the explicit closeout note.

Milestone 3 should still avoid turning into reporting, export, market-context, or analytics work that belongs to later milestones.

It should not expand into broker integration, market data ingestion, a web UI, or broader analytics work.

## Milestone 4

Milestone 4 should add read-only market and context support for planning and review.

Focus areas:

- read-only retrieval of selected market and context inputs
- local snapshot or caching behavior for later review
- CLI access to context alongside planning and review workflows
- explicit source-of-truth separation between external context and internal trade meaning

It should not expand into live streaming, execution triggers, broker coupling, or automated trading behavior.

## Milestone 5

Milestone 5 should deepen post-trade learning and local operational robustness.

Focus areas:

- review tagging and filtering
- narrow reporting and export workflows
- local backup and operational support for the local-first workflow
- journal-grade summaries rather than portfolio-platform analytics

It should not expand into portfolio-engine behavior, cloud-first operations, AI-generated review content, or reinforcement learning.

## Explicit Deferrals

The accepted roadmap still defers:

- Postgres as the active backend
- broker integration
- FastAPI
- reinforcement learning
- live automation

Reinforcement learning remains exploratory knowledge only. It is not the accepted Milestone 3 direction.

## Related Pages

- [[milestone-2-roadmap]]
- [[milestone-3-closeout]]
- [[application-implementation-status]]
- [[development-workflow]]
- [[data-and-platform-strategy]]
