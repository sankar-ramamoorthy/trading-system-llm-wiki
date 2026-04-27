---
title: Application Repo Documentation Sources
type: topic
status: active
tags: [trading-system, documentation, adr, application-repo]
created: 2026-04-19
updated: 2026-04-27
---

# Application Repo Documentation Sources

The application repository keeps versioned design documents and ADRs beside the code they govern. The knowledge base should treat those files as primary sources, then synthesize their current meaning into durable wiki pages.

## Primary Source Locations

Application repo:

```text
C:\Users\bosto\dockerstuff\trading-system
```

Primary architecture and design sources:

```text
C:\Users\bosto\dockerstuff\trading-system\DOCS\
C:\Users\bosto\dockerstuff\trading-system\DOCS\ADR\
```

Current observed files:

- `README.md`, updated 2026-04-27 to reflect Milestone 6 provider-boundary status
- `DOCS/domain-model.md` v2, dated 2026-04-19
- `DOCS/milestone-1-summary.md` v1, dated 2026-04-20
- `DOCS/milestone-2-roadmap.md` v1, dated 2026-04-20
- `DOCS/milestones-3-to-5-roadmap.md`, accepted 2026-04-24
- `DOCS/milestone-4-market-context-design.md`, accepted-for-roadmap 2026-04-24
- `DOCS/milestone-5-review-learning-and-local-ops-design.md`, accepted-for-roadmap 2026-04-24
- `DOCS/milestone-6-market-data-provider-design.md`, accepted-for-roadmap 2026-04-27
- `DOCS/systems-blueprint.md` v2, dated 2026-04-19
- `DOCS/ADR/001-system-architecture.md`
- `DOCS/ADR/002-rules-vs-context.md`
- `DOCS/ADR/003-development-and-deployment-strategy.md`
- `DOCS/ADR/004-canonical-domain-and-source-of-truth.md`
- `DOCS/ADR/005-mvp-definition-and-boundaries.md`, accepted 2026-04-20
- `DOCS/ADR/006-deferred-learning-systems-boundary.md`, accepted 2026-04-26
- `DOCS/ADR/007-market-data-provider-boundary.md`, accepted 2026-04-27

Supplemental implementation sources used for the 2026-04-24 synthesis:

- raw and processed Milestone 2 issue notes in this knowledge base for Issues 9, 10, and 11
- raw notes captured after Issue 12 through Issue 14 implementation
- application repo source files under `src/trading_system/`
- application repo tests under `tests/`

Note: the current application repo filename is `systems-blueprint.md`, plural.

## Source of Truth Roles

The application repo docs are the versioned source for decisions tied to code history. They answer when a decision was made and what code version it governed.

The application repo `README.md` is a current-state source. It should be used to understand the implemented workflow, local commands, user-facing framing, and explicit non-scope, but it should not replace ADRs or the domain model as the decision record.

ADR-005 is the source of truth for the Milestone 1 MVP boundary. ADR-006 is the source of truth for deferring AI, ML, and reinforcement-learning systems. ADR-007 is the source of truth for the Milestone 6 market data provider boundary.

As of the later 2026-04-24 repo state, the README and roadmap docs are aligned with the current Milestone 2 implementation and with the accepted next milestone set. When docs and code disagree in future, prefer code and tests for implementation status and docs for intent and boundary.

The knowledge base is the living synthesis. It should summarize current meaning, maintain cross-links, and reconcile multiple source documents into canonical pages such as [[architecture-overview]], [[canonical-domain-model]], and [[deterministic-rules-vs-contextual-intelligence]].

Do not blindly duplicate repo ADRs into the wiki. Use them as raw sources and promote their implications into entity, topic, index, and ADR pages where useful.

## Sync Workflow

When an ADR or design doc changes in the application repo:

1. Read the changed file in `DOCS/` or `DOCS/ADR/`.
2. Compare it with existing wiki pages.
3. Update affected entity, topic, index, or knowledge ADR pages.
4. Record contradictions or supersessions explicitly.
5. Leave implementation-specific details in the application repo unless they explain a durable design decision.

## README Ownership

The application repo `README.md` belongs in the application repo and should be updated by the repo agent or developer. It should summarize the current codebase and status, but it should verify project purpose, architecture, and core principles against this knowledge base before changing them.

In practice:

- knowledge base pages define long-term project intent and current synthesis
- application repo `README.md` presents the current implementation state
- application repo `DOCS/ADR/` preserves versioned decision history

## Related Pages

- [[knowledge-base-workflow]]
- [[development-workflow]]
- [[architecture-overview]]
- [[canonical-domain-model]]
- [[application-implementation-status]]
- [[mvp-definition-and-boundaries]]
- [[milestone-2-roadmap]]
