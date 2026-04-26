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

Milestone 4: read-only market context.

## Active Focus

- Refine the implemented local JSON context snapshot workflow for planning and review.
- Preserve the manual, local, auditable workflow established through Milestones 1 through 3.
- Keep deterministic rules separate from advisory context and external data.
- Keep external market-data providers deferred until a provider-boundary ADR is accepted.

## Current Concerns

- Do not turn context ingestion into broker execution, automation, or trade decision delegation.
- Do not let external market data become the source of truth for trade meaning.
- Keep the knowledge base focused on durable synthesis, not raw implementation churn.

## Open Questions

- What follow-on context types are useful after the first local snapshot import slice?
- What minimum read-only context is useful before adding review and learning workflows in Milestone 5?

## High-Priority Links

- [[trading-system-index]]
- [[application-implementation-status]]
- [[knowledge-base-workflow]]
- [[milestones-3-to-5-roadmap]]
- [[milestone-4-context-snapshot-workflow]]
- [[application-repo-documentation-sources]]

## Update Rule

Update this file only when active milestone focus, current project direction, or high-priority navigation changes. If it grows beyond one screen, move detail into a canonical topic page and link to it here.
