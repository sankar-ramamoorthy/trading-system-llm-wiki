# Trading System LLM Wiki

This repository is the Markdown-first LLM wiki for the `trading-system` project. It is persistent project memory: the place to preserve architecture rationale, canonical concepts, processed notes, prompts, and AI-assisted synthesis around the trading system.

This is not the runtime application repo. The active application codebase is expected at:

```text
C:\Users\bosto\dockerstuff\trading-system
```

Use this wiki to preserve the why behind that codebase, not to run the trading system.

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

The wiki follows a simple LLM-wiki workflow:

```text
Capture raw material
        |
        v
Structure or process it
        |
        v
Promote stable conclusions
        |
        v
Maintain canonical pages and indexes
```

Raw notes should stay raw until reviewed. Recurring patterns, accepted decisions, and durable concepts can be promoted into topics, entities, indexes, or ADRs.

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
