# Trading System LLM Wiki

This repository is the LLM wiki for the `trading-system` project: a persistent design and coordination layer that keeps project intent stable across human and AI-assisted development sessions.

It preserves architecture rationale, canonical concepts, unresolved design tensions, rejected alternatives, prompts, and AI-assisted synthesis around the trading system.

This is not the runtime application repo. The active application codebase is expected at:

```text
C:\Users\bosto\dockerstuff\trading-system
```

Use this wiki to preserve the why behind that codebase, not to run the trading system.

## Why This Exists

Codex sessions are temporary. The project is not.

This wiki gives the project a durable context system:

- continuity across AI sessions
- architecture memory and source-of-truth boundaries
- stable domain vocabulary
- a place to explore design ideas before they become ADRs or code
- a traceable record of why a decision exists

During development, this wiki is upstream of code as an intent and reasoning layer. The application repo remains the execution surface and must still be checked before durable conclusions or implementation changes are accepted.

## Current Wiki Status

The status of this LLM wiki is tracked separately in [STATUS.md](STATUS.md).

`PROJECT.md` tracks short current-context and milestone focus for the trading-system project. `STATUS.md` tracks the operating state of this knowledge base itself.

## Key Files

- [PROJECT.md](PROJECT.md) - short active project context and high-priority links for the trading-system project.
- [STATUS.md](STATUS.md) - current status, capabilities, and maintenance concerns for this LLM wiki.
- [AGENTS.md](AGENTS.md) - repository rules for agents working in this knowledge base.
- [knowledge/index/trading-system-index.md](knowledge/index/trading-system-index.md) - main wiki navigation index.
- [knowledge/topics/knowledge-base-workflow.md](knowledge/topics/knowledge-base-workflow.md) - knowledge ingestion and processing workflow.
- [knowledge/topics/development-workflow.md](knowledge/topics/development-workflow.md) - AI-assisted development workflow context.

## Repository Layout

- `adr/` - knowledge-base architecture decisions and workflow decisions.
- `knowledge/raw/` - unprocessed notes, brainstorms, feedback, imports, and dated working context.
- `knowledge/processed/` - cleaned or processed source notes and summaries.
- `knowledge/entities/` - canonical entity pages.
- `knowledge/topics/` - synthesized topic pages and durable project knowledge.
- `knowledge/index/` - navigation and index pages.
- `knowledge/outputs/` - generated answers or temporary synthesized outputs.
- `prompts/` - restart prompts and reusable context for future sessions.
- `skills/` - repo-local skills that future agents can use after reading their `SKILL.md`.

## Working Model

The wiki acts as a pre-ADR sandbox and design pipeline:

```text
Idea or friction
        -> raw note
        -> processed synthesis
        -> topic, entity, prompt, or ADR
        -> application repo alignment
        -> code
```

Raw notes are flexible thinking space. Canonical pages are durable project memory. ADRs and code require reconciliation with the application repo.

## Promotion Rule

Promote a note only when it has been reviewed for durable value, reconciled against existing source-of-truth material when practical, and assigned to the correct target artifact.

Use this default path:

```text
knowledge/raw/
        -> knowledge/processed/
        -> knowledge/topics/, knowledge/entities/, knowledge/index/, adr/, prompts/, or application repo work
```

Do not treat raw notes as canonical. Do not start implementation from raw feedback alone unless the user explicitly asks for a tactical change.

See [knowledge/topics/knowledge-base-workflow.md](knowledge/topics/knowledge-base-workflow.md) for the full promotion guidance.

## Knowledge Rules

- Keep generated output separate from canonical knowledge unless promotion is explicit.
- Preserve contradictions and call them out rather than silently replacing prior context.
- Prefer `[[wikilinks]]` for durable internal wiki pages.
- Keep diagrams ASCII-only.
- Verify against the linked application repo when synthesizing durable knowledge from implementation notes.

## Repo-Local Skills

Project-specific skills may live under `skills/`. They are not global Codex skills by default.

When asking an agent to use one, refer to it by name, for example:

```text
use structure-feedback-note on this raw feedback note
```

The agent should read `skills/<skill-name>/SKILL.md` before acting.
