# CLAUDE.md — trading-system Knowledge Base

This file tells Claude Code how to work in this repository.
Read this before acting on any instruction.

---

## What This Repo Is

This is the **LLM wiki** for the `trading-system` project.
It is a persistent design and coordination layer — not the runtime application.

The active application codebase is at:

```
C:\Users\bosto\dockerstuff\trading-system
```

This repo preserves **why** decisions were made, not how to run the system.

---

## What Claude Should Do Here

- Capture, process, and promote knowledge artifacts
- Synthesize raw notes into canonical topic, entity, index, or ADR pages
- Preserve contradictions — call them out, never silently overwrite
- Maintain cross-links between pages using `[[wikilinks]]` style
- Keep generated output in `knowledge/outputs/` unless explicit promotion is requested
- Read repo-local skills under `skills/<skill-name>/SKILL.md` when asked to use one

---

## What Claude Must NOT Do Here

- Do not treat raw notes as canonical
- Do not start implementation work from this repo
- Do not silently replace prior context with new notes
- Do not invent ADRs or entity definitions not grounded in existing material
- Do not write code unless explicitly asked to produce a code artifact for knowledge purposes
- Do not modify anything in the linked application repo from here

---

## Promotion Rule

Raw notes become canonical only after explicit review and promotion:

```
knowledge/raw/
    -> knowledge/processed/
    -> knowledge/topics/, knowledge/entities/, knowledge/index/, adr/, or prompts/
```

Do not promote automatically. Wait for the user to confirm promotion intent.

---

## Key Files

| File | Purpose |
|---|---|
| `PROJECT.md` | Active milestone focus and high-priority links |
| `STATUS.md` | Wiki operating state and maintenance concerns |
| `AGENTS.md` | Repository rules (shared with Codex) |
| `knowledge/index/trading-system-index.md` | Main navigation index |
| `knowledge/topics/application-implementation-status.md` | Synthesized milestone status |
| `knowledge/topics/knowledge-base-workflow.md` | Ingestion and promotion workflow |

---

## Directory Layout

| Path | Contents |
|---|---|
| `adr/` | Knowledge-base architecture decisions |
| `knowledge/raw/` | Unprocessed notes and imports |
| `knowledge/processed/` | Cleaned source notes |
| `knowledge/entities/` | Canonical entity pages |
| `knowledge/topics/` | Synthesized topic pages |
| `knowledge/index/` | Navigation pages |
| `knowledge/outputs/` | Generated or temporary outputs |
| `prompts/` | Restart prompts and reusable session context |
| `skills/` | Repo-local skills |

---

## Formatting Rules

- Markdown only
- ASCII diagrams only — no Mermaid, no images
- Use `[[wikilinks]]` for internal durable references
- Use YAML front matter for canonical pages:

```yaml
---
title: Example Title
type: topic
status: active
tags: [trading-system]
created: YYYY-MM-DD
updated: YYYY-MM-DD
---
```

- Dated raw notes: `EOD Summary 20260418.md`
- Canonical pages: stable lowercase names like `domain-model.md`

---

## Primary Source Material

When synthesizing durable knowledge, treat these as authoritative:

- `C:\Users\bosto\dockerstuff\trading-system\DOCS\`
- `C:\Users\bosto\dockerstuff\trading-system\DOCS\ADR\`
- `C:\Users\bosto\dockerstuff\trading-system\STATUS.md`
- `C:\Users\bosto\dockerstuff\trading-system\README.md`

If a raw note contradicts the application repo or existing canonical pages,
**call out the contradiction** rather than silently replacing prior context.

---

## Repo-Local Skills

Skills live under `skills/<skill-name>/SKILL.md`.
When the user asks to use one by name, read its `SKILL.md` first, then follow its instructions.
Do not install repo-local skills globally unless explicitly asked.

---

## Session Start Checklist

1. Read `PROJECT.md` for active milestone focus
2. Read `STATUS.md` for current wiki state
3. Check `knowledge/index/trading-system-index.md` for navigation
4. Read any skill `SKILL.md` before using a repo-local skill
5. Reconcile any new material against application repo sources before treating it as canonical

---

## Relationship to AGENTS.md

`AGENTS.md` contains durable repository rules shared with Codex.
`CLAUDE.md` (this file) contains Claude Code-specific operating instructions.
When they conflict, prefer `CLAUDE.md` for Claude Code sessions.
