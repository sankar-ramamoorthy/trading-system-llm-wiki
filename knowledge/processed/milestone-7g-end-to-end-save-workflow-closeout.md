---
title: Milestone 7G End-to-End Save Workflow — Closeout
type: processed
status: active
tags: [trading-system, milestone-7, 7G, trade-capture, acceptance, closeout]
created: 2026-05-02
updated: 2026-05-02
source: knowledge/raw/Milestone 7G is complete.md
---

# Milestone 7G End-to-End Save Workflow — Closeout

Processed from `knowledge/raw/Milestone 7G is complete.md` on 2026-05-02.

## Result

7G is complete. All acceptance criteria met.

## What Was Validated

- `docker compose up --build` — both containers healthy
- Parse → `qwen/qwen3-32b` via Groq correctly extracts fields and surfaces validation issues
- Save → creates linked TradeIdea, TradeThesis, TradePlan records
- Persist → confirmed in `store.json` with `approval_state: draft`
- All error paths: empty input, missing fields, ambiguous fields, unknown symbol, unknown plan ID
- `uv run pytest`: 216 passed

## Infrastructure Fixes Made During Acceptance

These were not planned in the 7G scope but were required to complete the acceptance run:

- **LLM provider switch**: Ollama llama3.1 was unavailable; switched to Groq `qwen/qwen3-32b` (60 RPM free tier).
- **API key wiring**: Added `env_file` to `docker-compose.yml` so `GROQ_API_KEY` reaches the backend container.
- **LiteLLM parser hardening**: Parser now tolerates small-model output variance — specifically string-as-list and non-string candidate values.

Note: 7A had set up Host Ollama placeholders. This acceptance run confirmed that the parser boundary (7D) correctly abstracts the LLM provider — switching from Ollama to Groq required only configuration and minor parser tolerance changes, not domain or API changes.

## Next

Milestone 7H — Milestone Closeout (tests, docs, roadmap updates, KB synthesis).

## Related Pages

- [[milestone-7-api-first-trade-capture-issue-map]]
- [[application-implementation-status]]
