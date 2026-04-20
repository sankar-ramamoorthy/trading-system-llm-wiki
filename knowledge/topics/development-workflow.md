---
title: Development Workflow
type: topic
status: active
tags: [trading-system, workflow, development]
created: 2026-04-19
updated: 2026-04-20
---

# Development Workflow

Development should be issue-based, milestone-driven, and tied to explicit acceptance criteria.

## Tooling Stance

The intended working setup includes:

- GitHub Desktop for day-to-day Git workflow
- Git worktrees for task or agent isolation
- Codex CLI and Claude CLI as development and knowledge assistants
- VS Code as the primary editing environment
- Docker for runtime infrastructure, not for AI tools

## Process Rules

- Do not write code without an explicit issue.
- Use architecture and design issues as well as implementation issues.
- Keep work organized by milestones or phases.
- Avoid premature renaming; `trading-system` remains the project name for now.
- Keep code changes tied to acceptance criteria.

## Early Build Direction

The early runtime should favor local Python development with `uv`, `pyproject.toml`, and a Typer CLI. After the in-memory Milestone 1 demo, the next persistence step can use SQLite or similar durable local storage if that keeps the workflow simple; Postgres in Docker remains an option when stronger database behavior is justified. FastAPI is acceptable later when an API layer is needed, but it should call the same service layer as the CLI.

## Knowledge Workflow

Raw notes enter `knowledge/raw/`. Codex or Claude should process them into durable pages under `knowledge/entities/`, `knowledge/topics/`, and `knowledge/index/`, then move processed source material into `knowledge/processed/`.

Use the knowledge base as long-term project memory. When beginning coding work in the application repo, inject relevant ADRs, canonical pages, and prompts from this repository into the working context.

The application repo and knowledge-base repo should each keep a focused `AGENTS.md`: the application repo should guide building and testing, while this repo should guide ingestion, cross-linking, and preservation of project memory.

## Repo Docs and README

The application repo keeps active design docs and ADRs under `DOCS/` and `DOCS/ADR/`. Those files are primary sources for decisions tied to code history. When they change, the knowledge base should be updated as the living synthesis.

The application repo `README.md` is maintained in the application repo. It is also a status source for the knowledge base when it reports current workflow, local commands, and implemented scope. Before updating it, verify current status, project purpose, and core principles against the knowledge base.

The final 2026-04-20 README alignment records Milestone 1 as complete and frames the current system as a manual discipline and journaling tool. Current development should keep work issue-based and focus next on persistence and `OrderIntent` before external integrations or automation.

## Related Pages

- [[trading-system]]
- [[architecture-overview]]
- [[first-vertical-slice]]
- [[application-project-structure]]
- [[application-implementation-status]]
- [[knowledge-base-workflow]]
- [[application-repo-documentation-sources]]
