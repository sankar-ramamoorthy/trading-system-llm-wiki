---
title: Knowledge Base Workflow
type: topic
status: active
tags: [trading-system, knowledge-base, workflow, agents]
created: 2026-04-19
updated: 2026-04-19
---

# Knowledge Base Workflow

The knowledge base is the long-term memory for the trading system. The runtime code repository contains what the system does; this repository preserves why decisions were made, what alternatives were rejected, and how concepts should be interpreted.

## Separation of Labor

The application repository should be treated as the builder workspace. Its `AGENTS.md` should focus on build commands, tests, local project structure, coding standards, and implementation behavior.

The knowledge-base repository should be treated as the librarian workspace. Its `AGENTS.md` should focus on note ingestion, front matter, wikilinks, cross-linking, contradiction handling, and promotion from raw notes to canonical pages.

Keeping the two instruction files separate preserves role clarity and avoids loading irrelevant context into every session.

## Context Injection

When beginning implementation work in the application repository, include the relevant knowledge-base context:

- the application repo `AGENTS.md`
- relevant ADRs and design docs from the application repo `DOCS/` and `DOCS/ADR/`
- relevant knowledge-base ADRs from this repository
- relevant canonical entity and topic pages
- the current restart prompt, if the task resumes prior work

The purpose is to make coding sessions respect the durable architecture, domain vocabulary, and source-of-truth boundaries.

## Repo Documentation as Primary Sources

The application repo keeps its versioned design documents under:

```text
C:\Users\bosto\dockerstuff\trading-system\DOCS\
C:\Users\bosto\dockerstuff\trading-system\DOCS\ADR\
```

Treat those files as primary source material for high-level architecture and logic. The knowledge base should synthesize their implications instead of copying them wholesale.

See [[application-repo-documentation-sources]] for the current source map and sync workflow.

## Raw Note Promotion

Use `knowledge/raw/` as a scratchpad for unprocessed observations, edge cases, and imported discussion. Raw notes should eventually be promoted into:

- `knowledge/entities/` for canonical concepts and project entities
- `knowledge/topics/` for synthesized subject pages
- `knowledge/index/` for navigation and maps
- `adr/` for durable architectural decisions
- `prompts/` for reusable AI session context

After promotion, move the source note to `knowledge/processed/` and leave any durable interpretation in canonical pages rather than only in processing summaries.

## Traceability

Use the knowledge base to preserve intent:

- why a schema shape exists
- why a module boundary exists
- why an early feature was deferred
- whether a later implementation contradicts the original design
- which notes supersede older assumptions

When contradictions appear, call them out directly instead of silently deleting prior context.

## Agent Guidance

The knowledge-base `AGENTS.md` should mention the linked application repo:

```text
C:\Users\bosto\dockerstuff\trading-system
```

When practical, summaries of new raw notes should be checked against the application repo README, current code, and active project instructions before they are promoted.

The application repo README is owned by the application repo. It should be updated there after major design milestones, using this knowledge base as the source for project intent, current architecture, and core principles.

## Related Pages

- [[development-workflow]]
- [[trading-system-index]]
- [[application-repo-documentation-sources]]
