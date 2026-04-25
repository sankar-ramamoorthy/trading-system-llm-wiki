---
title: Brainstorm Capture Workflow
type: adr
status: accepted
tags: [ai, knowledge-system, brainstorming, workflow, trading-system]
created: 2026-04-25
---

# ADR 0002 - Brainstorm Capture Workflow

## Context

The trading-system knowledge base separates durable project memory from unprocessed notes. Running the application can surface friction, rough ideas, and possible design directions before they are ready to become issues, topic pages, or ADRs.

Brainstorming is useful but messy. It can contain contradictions, premature solutions, unclear observations, and ideas that should not become canonical knowledge too early. The user also should not need to remember front matter, filenames, or wiki conventions while capturing thoughts.

## Decision

Brainstorming is a first-class but temporary raw artifact.

Brainstorm notes should be captured in `knowledge/raw/` with:

- `type: brainstorm`
- `status: raw`
- a dated filename using `brainstorm-YYYYMMDD-short-topic.md`
- sections for trigger, raw input, observations, ideas, questions, concerns, and possible next outputs

A reusable Codex skill named `capture-brainstorm-note` will support this workflow. Its purpose is to turn messy notes, pasted chat output, or app-friction reports into structured raw brainstorm notes without promoting them to canonical knowledge.

The skill is installed outside the wiki repository under:

```text
C:\Users\bosto\.codex\skills\capture-brainstorm-note
```

Normal use does not require application code. The user can invoke it in Codex with prompts such as:

```text
Use $capture-brainstorm-note to turn this rough note into a structured brainstorm note in knowledge/raw/.
```

## Consequences

### Positive

- The user can capture ideas quickly without manually formatting Markdown front matter.
- Messy thinking remains available as evidence without polluting canonical pages.
- Later processing can promote only the durable conclusions into topics, entities, ADRs, issues, or `PROJECT.md`.
- The distinction between raw thinking and stable knowledge becomes explicit.

### Negative

- Raw brainstorm notes still need later review and processing.
- The skill depends on Codex skill discovery, so a new Codex session may be needed before it appears automatically.
- Overuse could create raw-note accumulation if brainstorms are never processed.

## Follow-Up Rules

- Do not update index pages, topic pages, entity pages, `PROJECT.md`, or `AGENTS.md` during brainstorm capture unless separately requested.
- Do not move brainstorm notes to `knowledge/processed/` until their useful content has been reviewed.
- Promote only conclusions, decisions, recurring themes, or accepted next actions.
- Preserve contradictions instead of silently resolving them during raw capture.

## Related Pages

- [[knowledge-base-workflow]]
- [[development-workflow]]
- [[trading-system-index]]
