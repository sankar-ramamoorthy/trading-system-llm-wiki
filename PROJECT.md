---
title: Project Brief
type: project
status: active
tags: [trading-system, project, context]
created: 2026-04-25
updated: 2026-05-02
---

# Project Brief

This file is the short current-context entrypoint for the trading-system LLM wiki. Keep it brief and link into canonical pages rather than duplicating them.

## Current Phase

Milestone 6 is complete. Milestone 7 has started under ADR-008 API-first web product and trade-capture draft workflow.

## Active Focus

- Treat Milestone 6 read-only market data provider integration as closed.
- Treat Milestone 7A Dockerized Runtime Foundation as complete.
- Treat Milestone 7B Reference Lookup Foundation as complete.
- Treat Milestone 7C Trade Capture Draft Contract as complete.
- Treat Milestone 7D Natural-Language Parser Boundary as complete.
- Treat Milestone 7E FastAPI Trade Capture Service as complete.
- Treat Milestone 7F React/Vite Trade Capture Workspace as complete.
- Treat Milestone 7G End-to-End Save Workflow as complete.
- Treat Milestone 7H Milestone Closeout as complete.
- Milestone 7 is fully closed. Milestone 8 direction is outcome-level until a narrow implementation slice is ready.
- Preserve ADR-007 and ADR-009: market data provider boundaries for yfinance and Massive.com daily OHLCV snapshots.
- Keep yfinance as the default provider and Massive.com as the first credentialed provider.
- Preserve the manual, local, auditable workflow established through Milestones 1 through 3.
- Treat Milestone 4 read-only market context as complete and keep it advisory, local-first, and non-canonical.
- Treat Milestone 5 review, export, and local JSON operations as complete.
- Keep deterministic rules separate from advisory context and external data.
- Keep external market data behind the `MarketContextSnapshot` boundary.

## Current Concerns

- Do not turn context ingestion into broker execution, automation, or trade decision delegation.
- Do not let external market data become the source of truth for trade meaning.
- Do not treat `yfinance` as production-grade market data infrastructure.
- Do not expand provider work beyond the accepted daily OHLCV/advisory context boundary without a new explicit issue or ADR update.
- Do not store API keys in snapshots, logs, docs examples, tests, or committed files.
- Do not put API-key collection or key-vault management into the 7F browser trade-capture workspace.
- Do not couple provider response objects or schemas to domain logic.
- Do not turn review tags into a taxonomy system, analytics platform, generated coaching, or review-editing workflow.
- Do not turn review quality scores or journal export into optimization, recommendations, or coaching.
- Keep Markdown export factual and metadata-only for linked context; full context payload inspection belongs in `show-context`.
- Keep local JSON operations explicit, local-first, and operational; do not turn them into cloud sync, scheduled backup automation, migrations, or a new persistence architecture.
- Keep the knowledge base focused on durable synthesis, not raw implementation churn.

## Open Questions

- Milestone 8 should stay outcome-level until Milestone 7 parse/edit/save scope is stable.
- Local encrypted API-key storage needs a later design discussion before it becomes an ADR or implementation milestone.
- A reusable local secret-vault library is plausible, but the first accepted shape should be library-first and small, not a full key-management product.

## High-Priority Links

- [[trading-system-index]]
- [[application-implementation-status]]
- [[knowledge-base-workflow]]
- [[milestones-3-to-5-roadmap]]
- [[milestone-4-context-snapshot-workflow]]
- [[milestone-5-review-tags-and-filtering]]
- [[milestone-5-review-quality-scores]]
- [[milestone-5-markdown-journal-export]]
- [[milestone-5-local-json-operations]]
- [[milestone-6-market-data-provider-boundary]]
- [[milestone-7-api-first-trade-capture-issue-map]]
- [[reusable-local-secret-vault-library]]
- [[api-first-trade-capture-product-vision]]
- [[application-repo-documentation-sources]]

## Update Rule

Update this file only when active milestone focus, current project direction, or high-priority navigation changes. If it grows beyond one screen, move detail into a canonical topic page and link to it here.
