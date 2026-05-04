---
title: Project Brief
type: project
status: active
tags: [trading-system, project, context]
created: 2026-04-25
updated: 2026-05-04
---

# Project Brief

This file is the short current-context entrypoint for the trading-system LLM wiki. Keep it brief and link into canonical pages rather than duplicating them.

## Current Phase

Milestones 1 through 15 are complete. The accepted next slice is Milestone 16 Finqual Fundamentals Provider.

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
- Milestone 7 is fully closed.
- Treat Milestone 8 Options Chain Ingestion as complete.
- Treat Milestone 9 Web Product Beyond First Capture as complete.
- Treat Milestone 10 Secure Credentials as complete.
- Treat Milestone 11 Broker Boundary and Paper Trading as complete.
- Treat Milestone 12 Paper Execution Hardening as complete.
- Treat Milestone 13 Alpaca Paper Adapter as complete.
- Treat Milestone 14 Broker Reconciliation And Status Sync as complete.
- Treat Milestone 15 Alpaca Read-Only Market Data Provider as complete.
- Treat Milestone 16 as the planned Finqual read-only fundamentals and ownership provider slice.
- Treat broker UI expansion as later work: Milestone 17 read-only API/web broker visibility, then Milestone 18 browser paper execution controls.
- See `DOCS/product-roadmap.md` for the accepted near-term sequence.
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
- Do not couple Alpaca market data provider work to Alpaca broker execution.
- Do not treat Finqual fundamentals, insider transactions, or 13F snapshots as canonical trade meaning.
- Do not add automatic provider fallback between yfinance, Massive.com, Alpaca, Finqual, or later providers.
- Do not store API keys in snapshots, logs, docs examples, tests, or committed files.
- Do not put API-key collection or key-vault management into the 7F browser trade-capture workspace.
- Do not couple provider response objects or schemas to domain logic.
- Do not turn review tags into a taxonomy system, analytics platform, generated coaching, or review-editing workflow.
- Do not turn review quality scores or journal export into optimization, recommendations, or coaching.
- Keep Markdown export factual and metadata-only for linked context; full context payload inspection belongs in `show-context`.
- Keep local JSON operations explicit, local-first, and operational; do not turn them into cloud sync, scheduled backup automation, migrations, or a new persistence architecture.
- Keep the knowledge base focused on durable synthesis, not raw implementation churn.

## Open Questions

- Broker-paper-trading work should preserve the source-of-truth boundary: broker facts are external execution facts, local JSON remains the source for internal trade records, and the trading system owns trade meaning.
- Future broker visibility should build on explicit reconciliation and mismatch reporting, but it now follows the Alpaca and Finqual read-only provider milestones.
- A reusable local secret-vault library remains plausible beyond the app-specific Milestone 10 implementation, but the accepted app shape is already library-first and small.

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
- [[milestone-9-web-product-beyond-first-capture]]
- [[milestone-10-secure-credentials]]
- [[milestone-11-broker-boundary-and-paper-trading]]
- [[milestone-14-broker-reconciliation-and-status-sync]]
- [[milestone-15-alpaca-read-only-market-data-provider]]
- [[post-milestone-11-roadmap]]
- [[reusable-local-secret-vault-library]]
- [[api-first-trade-capture-product-vision]]
- [[application-repo-documentation-sources]]

## Update Rule

Update this file only when active milestone focus, current project direction, or high-priority navigation changes. If it grows beyond one screen, move detail into a canonical topic page and link to it here.
