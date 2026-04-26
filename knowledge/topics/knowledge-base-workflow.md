---
title: Knowledge Base Workflow
type: topic
status: active
tags: [trading-system, knowledge-base, workflow, agents]
created: 2026-04-19
updated: 2026-04-26
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

## Promotion Rule

Promote a note only when it has been reviewed for durable value, reconciled against existing source-of-truth material when practical, and assigned to the correct target artifact.

Promotion targets:

- Keep material in `knowledge/raw/` when it is still an unprocessed observation, brainstorm, feedback note, pasted context, or rough idea.
- Move the original source note to `knowledge/processed/` after its useful content has been extracted, structured, or promoted.
- Promote recurring patterns, synthesized workflows, design concerns, and cross-cutting explanations to `knowledge/topics/`.
- Promote stable domain objects, named systems, project concepts, and canonical definitions to `knowledge/entities/`.
- Promote navigation maps and reader entrypoints to `knowledge/index/`.
- Promote durable architectural decisions, source-of-truth boundaries, workflow decisions, repo-structure decisions, and long-term constraints to `adr/`.
- Promote reusable AI session context to `prompts/`.
- Move toward application repo work only after the decision or design implication is clear and aligned with the application repo README, docs, ADRs, and current code.

Promotion checklist:

- The useful claim is more than a passing thought, or it captures a high-impact design concern.
- The target artifact is clear: topic, entity, ADR, prompt, application issue, or no action.
- Existing canonical pages and application repo docs have been checked when practical.
- Contradictions are recorded instead of silently resolved.
- The original source remains traceable through the processed note, links, or cited context.
- Implementation is not started from raw feedback alone unless the user explicitly asks for a tactical change.

This rule exists to prevent three failure modes: raw backlog, wiki-to-code drift, and raw notes becoming too authoritative before they have been reconciled.

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
