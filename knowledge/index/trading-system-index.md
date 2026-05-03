---
title: Trading System Knowledge Index
type: index
status: active
tags: [trading-system, index]
created: 2026-04-19
updated: 2026-05-03
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
- [[milestone-7-api-first-trade-capture-issue-map]] - issue map for the API-first trade-capture web product milestone
- [[milestone-9-web-product-beyond-first-capture]] - proposed plan-centered web workbench slice after first trade capture
- [[milestone-10-secure-credentials]] - completed local encrypted secret vault and CLI credential-resolution milestone
- [[milestone-11-broker-boundary-and-paper-trading]] - completed broker execution boundary and simulated paper-execution slice
- [[milestone-12-paper-execution-hardening]] - completed simulated paper execution hardening slice
- [[milestone-13-alpaca-paper-adapter]] - completed Alpaca paper adapter slice behind the broker port
- [[post-milestone-11-roadmap]] - staged roadmap for broker reconciliation, API/web visibility, browser controls, and real-money readiness
- [[reusable-local-secret-vault-library]] - discussion note for a possible reusable local secret-vault library and future ADR
- [[product-roadmap-and-learning-boundaries]] - near-term roadmap, long-term product direction, and deferred AI/RL boundary
- [[application-project-structure]] - Python modular monolith structure and boundaries
- [[application-implementation-status]] - current observed implementation status from app repo README, status notes, and source files
- [[knowledge-base-workflow]] - how the wiki supports the application repository
- [[application-repo-documentation-sources]] - map of application repo docs and ADRs used as primary sources
- [[wiki-runtime-boundary]] - foundational boundary between the runtime trading system and the LLM wiki; failure modes and integration rules
- [[feedback-to-design-pipeline]] - disciplined pipeline from self-observed friction to promoted topic, ADR, and code change
- [[cli-ux-friction]] - active log of CLI UX pain points; improvement options and natural language mode direction

## Current Design Center

Build a trade operating system where deterministic rules control risk and process, while a context-intelligence layer continuously checks whether the world still supports the trade.

## Processing Notes

Raw notes from 2026-04-18 were consolidated into the pages above and moved into `knowledge/processed/` on 2026-04-19.

Raw notes audit completed 2026-05-02: all 14 raw files reviewed. Three new topic pages promoted (`wiki-runtime-boundary`, `feedback-to-design-pipeline`, `cli-ux-friction`). Two archive notes created (`milestone-6-sequencing-rationale-20260427`, `roadmap-m8-through-m12-snapshot-20260502`). All remaining raw files marked `status: processed` in frontmatter. The raw directory now contains no unprocessed non-brainstorm notes.

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

Milestone 7 planning is recorded in [[milestone-7-api-first-trade-capture-issue-map]]. Issue 7A Dockerized Runtime Foundation is complete in the application repo as of 2026-04-29. It adds the Dockerized API/web runtime skeleton, FastAPI health endpoint, Vite React TypeScript frontend shell, and host Ollama configuration placeholders. Issue 7B Reference Lookup Foundation is also complete as of 2026-04-29. It adds seeded instrument lookup by symbol and playbook lookup by slug through the API. Trade capture, parser behavior, draft contracts, and save workflow remain later 7.x issues.

The non-brainstorm raw notes remaining on 2026-05-02 were processed into [[processing-summary-20260502-milestone-7-raw]]. Historical plans for yfinance ingestion, Milestone 7A, and the initial API-first trade-capture workspace were reconciled against the application repo. That pass established Milestone 6 as complete and Milestone 7A/7B as complete. Brainstorm notes were intentionally left in `knowledge/raw/`.

The later raw implementation note for Milestone 7C was processed into [[implemented-milestone-7c-20260502]]. The application repo now has the service-layer draft contract for parsed-but-unsaved trade capture state, with stable required/optional field definitions, missing and ambiguous field issue reporting, and save-readiness checks. That note made Milestone 7D the next planned slice, which was superseded by the later 7D implementation note.

The raw proposed plan for Milestone 7D was processed into [[proposed-milestone-7d-natural-language-parser-boundary-20260502]]. The plan kept 7D as a parser-boundary slice only: LiteLLM/Ollama adapter, parser port, fake parser, strict extraction behavior, and parser failure handling, without API endpoints, frontend UI, save workflow, persistence, recommendations, or claim verification. That planning status was superseded by the later 7D implementation note.

The later raw implementation note for Milestone 7D was processed into [[implemented-milestone-7d-20260502]]. The application repo now has the parser port, fake parser, LiteLLM-backed parser adapter, environment-based model/API-base configuration, JSON response validation, and mapping into the 7C draft contract. That note made Milestone 7E the next planned slice, which was superseded by the later 7E implementation note.

The raw proposed plan for Milestone 7E was processed into [[proposed-milestone-7e-fastapi-trade-capture-service-20260502]]. The plan kept 7E as a backend API slice: parse, save, and saved-result retrieval over existing parser, draft, reference lookup, planning, query, and JSON repository boundaries. That planning status was superseded by the later 7E implementation note.

The later raw implementation note for Milestone 7E was processed into [[implemented-milestone-7e-20260502]]. The application repo now has `TradeCaptureService`, `POST /trade-capture/parse`, `POST /trade-capture/save`, and `GET /trade-capture/saved/{trade_plan_id}` over local JSON-backed services. That note made Milestone 7F the next planned slice, which was superseded by the later 7F implementation note.

The raw notes describing what 7F should do were processed into [[proposed-milestone-7f-react-trade-capture-workspace-20260502]]. The durable direction was that 7F should replace the frontend status shell with a browser trade-capture workspace over the 7E backend. That planning status was superseded by the later 7F implementation note.

The raw implementation note for Milestone 7F was processed into [[implemented-milestone-7f-20260502]]. The application repo now has a React/Vite trade-capture workspace for raw trader-language input, parse, editable Idea/Thesis/Plan sections, field-level issue display, explicit save, and saved-result summary.

The Milestone 7G acceptance run was processed into [[implemented-milestone-7g-20260502]]. Docker/API end-to-end validation confirmed the full parse→save→persist workflow: Groq-backed `qwen/qwen3-32b` model wired in, `env_file` added to docker-compose for secret passing, LiteLLM parser hardened for small-model output variance, linked records confirmed in local JSON store, and 216 tests passing.

The Milestone 7H closeout was processed into [[implemented-milestone-7h-20260502]]. The milestone-7 closeout document was created, the README was updated with the web interface section, the issue map and STATUS.md were marked complete, and 216 tests + frontend build were confirmed passing. Milestone 7 is fully closed.

The post-Milestone 7 elevator pitch and capability baseline were processed into [[system-capability-after-milestone-7-20260502]]. This is the canonical capability snapshot as of 2026-05-02: two entry points (web + CLI), 13 capability areas, and explicit "does not do" boundaries.

The initial Milestone 8 scope notes (web product direction) were processed into [[milestone-8-initial-scope-notes-20260502]] and marked superseded. Options Chain Ingestion is now accepted as Milestone 8; the web product expansion moved to Milestone 9. The near-term sequence is now M8 (options chain) → M9 (web depth) → M10 (key vault) → M11 (paper trading). See `DOCS/product-roadmap.md`.

The API key/key vault discussion was processed into [[api-key-vault-discussion-20260502]] as design discussion input only. Key-vault work is not accepted 7F scope. The current stance remains environment-variable based configuration, with a possible later ADR or milestone for encrypted local key storage.

The reusable key-vault library idea was captured as a raw brainstorm note and promoted to [[reusable-local-secret-vault-library]] as an ADR candidate topic. The durable stance is library-first: reusable encrypted local storage and secret resolution, with project-specific CLI/API/UI integration left outside the reusable core.

Milestone 8 (Options Chain Ingestion) was implemented and processed into [[implemented-milestone-8-20260502]]. yfinance and Massive.com options chain adapters were added, `fetch-options-chain` CLI command wired in, and 233 tests pass. Massive.com options require a paid plan; yfinance works on the free tier. Milestone 9 (web product depth) is next.

A detailed encryption and master-key brainstorm was captured in [[key-vault-encryption-brainstorm-20260502]]. It covers Fernet vs AES-GCM vs OS keychain vs Age encryption, master-key management options, secret resolution precedence, CLI commands, and Docker behavior. The current roadmap assigns key vault work to Milestone 10 Secure Credentials, after Milestone 9 web product depth.

The raw Milestone 9 web product plan was processed into [[proposed-milestone-9-web-product-beyond-first-capture-20260502]] and promoted to [[milestone-9-web-product-beyond-first-capture]]. The promoted scope is a plan-centered browser workbench: saved plan list/detail, draft-plan approval, and metadata-only context attachment by copying existing snapshots to plans. It is planning material, not an implementation closeout.

Application repo status on 2026-05-03 now marks Milestones 9 and 10 complete. Milestone 9 added browser plan list/detail, draft approval, and context attachment. Milestone 10 added ADR-010, local encrypted secret vault CLI commands, OS keychain-backed master-key storage, and vault-first Massive.com credential resolution. The key-vault brainstorm was processed into [[milestone-10-secure-credentials]] and marked stale where its milestone numbering differed from the accepted app roadmap.

Milestone 11 is complete in the linked application repo. The roadmap direction for broker boundary and paper trading was implemented through ADR-011, core broker services, local `BrokerOrder` persistence, simulated paper broker execution, broker-linked fills, and CLI submit/sync/show commands. The durable boundary remains that broker facts are external execution facts while local records preserve internal trade meaning.

The raw Proposed Milestone 11 plan was processed on 2026-05-03 into [[proposed-milestone-11-broker-boundary-paper-execution-20260503]] and used to update [[milestone-11-broker-boundary-and-paper-trading]]. That planned stance was later implemented: core services and CLI only, simulated paper broker first, Alpaca-ready port but no live Alpaca calls, existing local position required before broker fills can be imported, and API/web execution controls deferred.

Follow-up on 2026-05-03 confirmed the ADR sequencing gap was resolved in the linked application repo. ADR-011 now exists, the Milestone 11 issue map marks 11A through 11E complete, and application `STATUS.md` records Milestone 11 complete with simulated CLI-only paper execution.

The raw Post-Milestone 11 Higher-Level Roadmap note was processed on 2026-05-03 into [[post-milestone-11-higher-level-roadmap-20260503]] and promoted to [[post-milestone-11-roadmap]]. M12 paper execution hardening and M13 Alpaca paper adapter are complete. The staged direction is now M14 broker reconciliation and status sync, M15 API/web broker visibility, M16 browser paper execution controls, with real-money execution treated as a readiness gate rather than a default milestone.

Milestones 12 and 13 are now complete in the linked application repo. Milestone 12 added broker-order query/list/detail hardening, simulated cancel/reject outcomes, and stronger CLI audit visibility with 264 full-suite tests passing. Milestone 13 added `AlpacaPaperBrokerClient`, `alpaca-py`, vault/env Alpaca credential resolution, `submit-paper-order --provider alpaca`, and Alpaca sync through existing CLI commands with 280 full-suite tests passing. The raw Milestone 13 implementation note was processed into [[implemented-milestone-13-alpaca-paper-adapter-20260503]] and promoted to [[milestone-13-alpaca-paper-adapter]].
