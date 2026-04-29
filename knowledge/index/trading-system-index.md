---
title: Trading System Knowledge Index
type: index
status: active
tags: [trading-system, index]
created: 2026-04-19
updated: 2026-04-27
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
- [[milestone-3-closeout]] - explicit closeout note for the manual workflow usability milestone
- [[milestones-3-to-5-roadmap]] - accepted roadmap after Milestone 2 for manual workflow usability, read-only market context, and review/local operations
- [[milestone-4-context-snapshot-workflow]] - initial local context snapshot workflow and provider deferral for Milestone 4
- [[milestone-5-review-tags-and-filtering]] - first Milestone 5 review/learning slice for creation-time tags and review filtering
- [[milestone-5-review-quality-scores]] - second Milestone 5 review/learning slice for optional process, setup, execution, and exit scores
- [[milestone-5-markdown-journal-export]] - third Milestone 5 review/learning slice for factual Markdown exports of reviewed trades
- [[milestone-5-local-json-operations]] - fourth Milestone 5 local-operations slice for JSON store validation, backup, and restore
- [[milestone-6-market-data-provider-boundary]] - accepted ADR-007 boundary for prototype yfinance daily OHLCV snapshots
- [[product-roadmap-and-learning-boundaries]] - near-term roadmap, long-term product direction, and deferred AI/RL boundary
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

Raw notes from 2026-04-22 through 2026-04-24 were processed on 2026-04-24. They record Milestone 2 implementation of durable local JSON persistence, read-only retrieval workflows, narrow `OrderIntent`, and later CLI workflow commands. They also record that the accepted post-Milestone-2 roadmap is now Milestones 3 through 5 for manual workflow usability, read-only market context, and review/local operations, while RL remains exploratory only.

Later raw notes on 2026-04-24 show early Milestone 3 implementation work: review inspection commands and read-command output consistency. The current synthesis is that Milestone 2 looks functionally complete but not yet formally closed out, while Milestone 3 has started.

Later raw notes also record the implemented Issue 17 usability bundle: thesis inspection commands, exact-match filters, stable sort modes, and README alignment. A separate proposal for `OrderIntent` cancellation remains deferred and non-canonical for now.

The later raw note for explicit order-intent cancellation supersedes that earlier planning-only status. Cancellation is now implemented as a narrow Milestone 3 follow-on and should be treated as current application behavior.

The knowledge base now records Milestone 3 as complete through an explicit closeout note, with Milestone 4 as the next active milestone.

The raw Milestone 4 initial context snapshot workflow plan from 2026-04-26 was processed into [[milestone-4-context-snapshot-workflow]]. The promoted durable direction is local JSON context snapshot import first, with external providers such as `yfinance` deferred behind a provider boundary and requiring an ADR when introduced.

The later raw implementation note for the Milestone 4 local context snapshot slice was processed on 2026-04-26. The application repo now implements `MarketContextSnapshot`, local JSON import, snapshot repositories, import/query services, and CLI commands for `import-context`, `list-context`, and `show-context`. Verification recorded 13 focused market-context tests passing and 117 total application tests passing.

The raw plan and implementation notes for surfacing market context in detail views were processed on 2026-04-26. The application repo commit `d9af758` adds metadata-only `Market context` sections to `show-trade-plan`, `show-position`, and `show-trade-review`, while keeping full payload inspection in `show-context`. The implementation note recorded 55 focused tests passing and 123 full-suite tests passing.

The raw decision, plan, and implementation notes for context discovery and copy workflow were processed on 2026-04-26. The application repo commit `4f5b0f0` adds broad `list-context` discovery filters and `copy-context`, which creates a new immutable linked snapshot instead of mutating the original. The implementation note recorded 43 focused context/read tests passing and 129 full-suite tests passing.

Milestone 4 is now closed in the application repo. The app repo status and summary docs mark read-only market context complete, with 43 focused market-context/read-model tests and 129 full-suite tests recorded at closeout. External provider integration remains deferred.

The first Milestone 5 slice is implemented in the application repo: creation-time review tags and review filtering. The slice adds `TradeReview.tags`, `create-trade-review --tag`, tag display in review list/detail output, and `list-trade-reviews --tag`, while explicitly avoiding review editing, taxonomy management, generated coaching, reporting/export, and broader analytics. Verification recorded 58 focused tests passing and 131 full-suite tests passing.

The Perplexity assessment based on the application repo README and STATUS was processed on 2026-04-26 and moved to `knowledge/processed/`. It did not change the accepted roadmap, but it reinforced durable guardrails: keep the daily workflow fast, keep market context advisory, avoid overbuilding, preserve meaningful review prompts, and continue testing lifecycle invariants.

The second Milestone 5 slice is implemented in the application repo: optional review quality scores. The slice adds `process_score`, `setup_quality`, `execution_quality`, and `exit_quality` to `TradeReview`, exposes create/list/show CLI support, and keeps the feature as creation-time review metadata rather than review editing, reporting, generated coaching, or analytics. Verification recorded 59 focused tests passing and 132 full-suite tests passing.

The third Milestone 5 slice is implemented in the application repo: Markdown journal export for reviewed trades. The slice adds `export-review-journal --output <path>`, reuses review filters, writes one section per matching review, refuses existing output files without `--overwrite`, leaves empty results unwritten, and keeps full context payload inspection isolated to `show-context`. Verification recorded 49 focused tests passing and 142 full-suite tests passing.

The fourth Milestone 5 slice is implemented in the application repo: local JSON operations for the configured store. The slice adds `validate-store`, `backup-store`, and `restore-store <backup-path> --overwrite`; backups are exact timestamped JSON copies, and restore validates backup files before replacing the active store. Verification recorded 67 focused persistence/CLI/retrieval tests passing and 156 full-suite tests passing.

Milestone 6 is complete in the application repo. ADR-007 accepts optional prototype-grade `yfinance` as the first provider stance and daily OHLCV history as the first data shape, with all provider output stored as advisory, non-canonical `MarketContextSnapshot` records. Milestone 6A implements yfinance daily OHLCV snapshots. Milestone 6B adds explicit provider selection through a registry boundary. ADR-009 accepts Massive.com as the first credentialed provider candidate. Milestone 6C implements `fetch-market-data --provider massive` with `MASSIVE_API_KEY`. Milestone 6D closes the milestone with 177 full-suite tests passing on 2026-04-29.
