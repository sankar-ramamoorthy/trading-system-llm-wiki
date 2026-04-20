---
title: Trading System Knowledge Index
type: index
status: active
tags: [trading-system, index]
created: 2026-04-19
updated: 2026-04-20
---

# Trading System Knowledge Index

This index is the navigation entry point for the trading-system knowledge base.

## Core Pages

- [[trading-system]] - project purpose, scope, and principles
- [[canonical-domain-model]] - core entities and source-of-truth boundaries
- [[architecture-overview]] - layered modular monolith architecture
- [[deterministic-rules-vs-contextual-intelligence]] - central separation between enforceable rules and advisory context
- [[context-intelligence-layer]] - thesis, regime, watchlist, and context monitoring
- [[trade-lifecycle-and-objects]] - idea-to-review lifecycle and structured trade objects
- [[data-and-platform-strategy]] - external platforms, data providers, and adapter stance
- [[development-workflow]] - issue-based, milestone-driven engineering workflow
- [[first-vertical-slice]] - first executable planned swing-trade workflow
- [[mvp-definition-and-boundaries]] - Milestone 1 MVP scope and explicit exclusions
- [[milestone-2-roadmap]] - post-MVP direction for persistence, retrieval, OrderIntent, P&L, and CLI usability
- [[application-project-structure]] - Python modular monolith structure and boundaries
- [[application-implementation-status]] - current observed implementation status from app repo README, status notes, and source files
- [[knowledge-base-workflow]] - how the wiki supports the application repository
- [[application-repo-documentation-sources]] - map of application repo docs and ADRs used as primary sources

## Current Design Center

Build a trade operating system where deterministic rules control risk and process, while a context-intelligence layer continuously checks whether the world still supports the trade.

## Processing Notes

Raw notes from 2026-04-18 were consolidated into the pages above and moved into `knowledge/processed/` on 2026-04-19.

Raw notes remaining on 2026-04-19 were consolidated into first-slice, application-structure, and knowledge-base workflow pages, then moved into `knowledge/processed/`.

Additional notes on 2026-04-19 clarified that application repo `DOCS/` and `DOCS/ADR/` are primary architecture sources, while this wiki is the synthesized long-term memory.

Application repo `DOCS/domain-model.md` and `DOCS/systems-blueprint.md` v2 updates from 2026-04-19 were synthesized into the canonical domain model, architecture overview, first vertical slice, trade lifecycle, rules/context, and source-documentation pages.

Raw status notes from 2026-04-19 and the updated application repo `README.md` were synthesized into the implementation status, first vertical slice, project structure, development workflow, and source-documentation pages.

Issue 4 through Issue 8 raw notes, the updated app README, `DOCS/milestone-1-summary.md`, `DOCS/milestone-2-roadmap.md`, and ADR-005 were synthesized on 2026-04-20. Milestone 1 is now recorded as complete as a local CLI-driven MVP vertical slice.

The final 2026-04-20 README alignment was synthesized into the project purpose, MVP boundary, implementation status, project structure, Milestone 2 roadmap, and source-documentation pages. It emphasizes the current system as a manual discipline and journaling tool, with persistence and `OrderIntent` as near-term post-MVP focus.
