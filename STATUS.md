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
- Synced the Milestone 6 market-data provider boundary and the implemented `fetch-market-data` slice into the project brief, implementation status, and milestone topic pages.
- Processed the Milestone 6C Issue 2 plan and implementation notes into a processed note and updated the canonical Milestone 6, roadmap, index, and project brief pages.
- Processed Milestone 6D closeout and updated project, roadmap, implementation status, index, and Milestone 6 topic pages to mark Milestone 6 complete.
- Processed the non-brainstorm raw Milestone 7 planning notes on 2026-05-02. Updated the Milestone 7 issue map, application implementation status, project brief, and index to reflect 7A/7B complete; this was superseded later the same day by the 7C implementation note.
- Processed the raw Milestone 7C implementation note on 2026-05-02. Updated canonical pages to mark 7C complete and 7D Natural-Language Parser Boundary as next; this was superseded later the same day by the 7D implementation note.
- Processed the raw proposed Milestone 7D parser-boundary plan on 2026-05-02. Updated the Milestone 7 issue map and index with the proposed 7D scope; this was superseded later the same day by the 7D implementation note.
- Processed the raw Milestone 7D implementation note on 2026-05-02. Updated canonical pages to mark 7D complete and 7E FastAPI Trade Capture Service as next; this was superseded later the same day by the 7E implementation note.
- Processed the raw proposed Milestone 7E FastAPI trade-capture service plan on 2026-05-02. Updated the Milestone 7 issue map and index with the proposed backend API scope; this was superseded later the same day by the 7E implementation note.
- Processed the raw Milestone 7E implementation note on 2026-05-02. Updated canonical pages to mark 7E complete and 7F React/Vite Trade Capture Workspace as next; this was superseded later the same day by the 7F implementation note.
- Processed the raw 7F scope note on 2026-05-02. Updated the Milestone 7 issue map and index with the proposed React/Vite trade-capture workspace flow; this was superseded later the same day by the 7F implementation note.
- Processed the API key/key vault discussion note on 2026-05-02 as discussion input only. Captured it as a later ADR/milestone candidate, not as accepted 7F scope.
- Captured the reusable local secret-vault library idea as a raw brainstorm note and added a durable discussion topic for a future ADR candidate.
- Processed the fuller raw Milestone 7F React/Vite workspace plan on 2026-05-02. Expanded the proposed 7F note and issue map with UI states, styling direction, validation, and scope boundaries; this was superseded later the same day by the 7F implementation note.
- Processed the raw Milestone 7F implementation note on 2026-05-02. Updated canonical pages to mark 7F complete and 7G End-to-End Save Workflow as next.
- Processed the raw proposed Milestone 7G end-to-end save workflow plan on 2026-05-02. Expanded the 7G section in the issue map with steps, scope boundaries, and verification criteria. Stored cleaned source note under `knowledge/processed/`.
- Processed the raw Milestone 7G implementation note on 2026-05-02. Updated canonical pages to mark 7G complete and 7H Milestone Closeout as next. Noted LLM provider switch from Ollama to Groq and parser hardening applied during acceptance.
- Processed the Milestone 7H closeout note on 2026-05-02. Updated canonical pages to mark Milestone 7 fully closed and recorded the final web/API validation.
- Processed the initial Milestone 8 scope notes and later roadmap snapshot on 2026-05-02. Reconciled the sequence to M8 Options Chain Ingestion, M9 Web Product Beyond First Capture, M10 Secure Credentials, and M11 Broker Boundary and Paper Trading.
- Processed the Milestone 8 implementation note on 2026-05-02. Updated canonical pages to mark options chain ingestion complete and Milestone 9 as the next active slice.
- Processed the raw Milestone 9 web product plan on 2026-05-02. Promoted a plan-centered browser workbench scope into a canonical topic page and archived the cleaned source note under `knowledge/processed/`.

## Next Maintenance Candidates

- Review recent raw and processed notes for promotion candidates.
- Decide whether feedback capture needs a knowledge-base ADR.
- Audit internal links from the main index into current topic pages.
- Clarify whether repo-local skills should remain project-local or later be promoted to global Codex skills.

## Update Rule

Update this file when the wiki's own structure, capabilities, workflows, maintenance risks, or repo-local tooling changes.

Do not use this file for trading-system milestone status, runtime implementation status, or application roadmap detail.
