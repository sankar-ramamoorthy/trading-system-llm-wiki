---
title: Project Brief
type: project
status: active
tags: [trading-system, project, context]
created: 2026-04-25
updated: 2026-04-26
---

# Project Brief

This file is the short current-context entrypoint for the trading-system LLM wiki. Keep it brief and link into canonical pages rather than duplicating them.

## Current Phase

Milestone 5: review, learning, and local operations.

## Active Focus

- Build on the third Milestone 5 slice: Markdown journal export for reviewed trades.
- Preserve the manual, local, auditable workflow established through Milestones 1 through 3.
- Treat Milestone 4 read-only market context as complete and keep it advisory, local-first, and non-canonical.
- Keep deterministic rules separate from advisory context and external data.
- Keep external market-data providers deferred until a provider-boundary ADR is accepted.

## Current Concerns

- Do not turn context ingestion into broker execution, automation, or trade decision delegation.
- Do not let external market data become the source of truth for trade meaning.
- Do not turn review tags into a taxonomy system, analytics platform, generated coaching, or review-editing workflow.
- Do not turn review quality scores or journal export into optimization, recommendations, or coaching.
- Keep Markdown export factual and metadata-only for linked context; full context payload inspection belongs in `show-context`.
- Keep the knowledge base focused on durable synthesis, not raw implementation churn.

## Open Questions

- What narrow local-operations slice should follow review tags, quality scores, and Markdown export?
- When should review editing or tag management be introduced, if ever?

## High-Priority Links

- [[trading-system-index]]
- [[application-implementation-status]]
- [[knowledge-base-workflow]]
- [[milestones-3-to-5-roadmap]]
- [[milestone-4-context-snapshot-workflow]]
- [[milestone-5-review-tags-and-filtering]]
- [[milestone-5-review-quality-scores]]
- [[milestone-5-markdown-journal-export]]
- [[application-repo-documentation-sources]]

## Update Rule

Update this file only when active milestone focus, current project direction, or high-priority navigation changes. If it grows beyond one screen, move detail into a canonical topic page and link to it here.
