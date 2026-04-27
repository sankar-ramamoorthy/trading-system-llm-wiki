# LLM Wiki Status

This file tracks the operating state of this LLM wiki, not the status of the `trading-system` runtime application.

For trading-system milestone focus, use [PROJECT.md](PROJECT.md). For agent operating rules, use [AGENTS.md](AGENTS.md).

## Current State

- Status: active
- Repository role: Markdown-first knowledge base for the `trading-system` project
- Runtime role: none
- Build system: none
- Test system: review-based Markdown checks
- Canonical navigation: [knowledge/index/trading-system-index.md](knowledge/index/trading-system-index.md)

## Current Capabilities

- Raw note capture under `knowledge/raw/`
- Processed note storage under `knowledge/processed/`
- Explicit promotion rule for moving raw ideas into durable wiki artifacts or application alignment
- Canonical topic, entity, and index structure under `knowledge/`
- Knowledge-base ADRs under `adr/`
- Restart prompts and reusable context under `prompts/`
- Global brainstorm capture skill documented by ADR 0002
- Repo-local feedback structuring skill under `skills/structure-feedback-note/`

## Maintenance Concerns

- Raw notes need periodic review so `knowledge/raw/` does not become a backlog.
- Processed notes should remain source material, not a substitute for canonical pages.
- Promotion decisions should preserve contradictions instead of overwriting prior context.
- Wiki intent must be reconciled against application repo docs and code before being treated as implementation truth.
- Repo-local skills depend on agents reading `AGENTS.md` and then `skills/<skill-name>/SKILL.md`.
- Wiki status should remain separate from trading-system application status.

## Recent Wiki Changes

- Clarified the README positioning around the wiki as a persistent design and coordination layer.
- Added an explicit promotion rule for moving notes from raw material into durable artifacts.
- Added structured brainstorm capture workflow and ADR.
- Added a repo-local `structure-feedback-note` skill.
- Added repo-local skill loading guidance to `AGENTS.md`.
- Added root `README.md` and this wiki-specific `STATUS.md`.
- Processed the Milestone 5 local JSON backup/restore plan and implementation notes into canonical wiki status and topic pages.

## Next Maintenance Candidates

- Review recent raw and processed notes for promotion candidates.
- Decide whether feedback capture needs a knowledge-base ADR.
- Audit internal links from the main index into current topic pages.
- Clarify whether repo-local skills should remain project-local or later be promoted to global Codex skills.

## Update Rule

Update this file when the wiki's own structure, capabilities, workflows, maintenance risks, or repo-local tooling changes.

Do not use this file for trading-system milestone status, runtime implementation status, or application roadmap detail.
