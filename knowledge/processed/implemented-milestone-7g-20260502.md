---
title: Implemented Milestone 7G — End-to-End Save Workflow
type: processed
status: active
tags: [trading-system, milestone-7, trade-capture, acceptance]
created: 2026-05-02
updated: 2026-05-02
---

# Implemented: Milestone 7G — End-to-End Save Workflow

## What Was Done

Milestone 7G is a Docker/API acceptance slice that validates the full parse→edit→save→persist workflow built in 7E (backend) and 7F (UI). No new domain features were added.

## Changes Made During Acceptance

### LLM Provider Switch

The default Ollama configuration (`ollama_chat/llama3.1`) was replaced with Groq:

- `TRADING_SYSTEM_LLM_MODEL=groq/qwen/qwen3-32b`
- `TRADING_SYSTEM_LLM_API_BASE=https://api.groq.com/openai/v1`
- `GROQ_API_KEY` added to `.env`

The originally targeted model (`moonshotai/kimi-k2-instruct`) is not available on the free Groq tier. `qwen/qwen3-32b` was chosen as the alternative with the same 60 RPM tier.

### docker-compose.yml

Added `env_file` to the api service so secrets in `.env` are passed into the container:

```yaml
env_file:
  - path: .env
    required: false
```

Without this, `GROQ_API_KEY` was available to Docker Compose for variable substitution but was not injected into the container environment.

### LiteLLM Parser Hardening

Two defensive fixes in `src/trading_system/infrastructure/litellm/trade_capture_parser.py`:

1. `_string_list`: small models sometimes return a scalar string where a list is expected. Now coerces a bare string to a single-element list.
2. `_ambiguous_issue` candidates: coerces non-string candidate values (or a bare string) instead of raising.

These fixes were surfaced by `lfm2.5-thinking:latest` (1.2B) before switching to qwen3-32b.

## Validation Recorded (2026-05-02)

- `docker compose up --build`: both api and web containers healthy
- `POST /trade-capture/parse` with NVDA swing text: fields extracted, issues surfaced correctly
- `POST /trade-capture/save` with complete draft: linked `TradeIdea`, `TradeThesis`, `TradePlan` created
- `GET /trade-capture/saved/{trade_plan_id}`: retrieved saved result summary correctly
- Local JSON store (`store.json`): confirmed all three linked records with `approval_state: draft`
- Error paths: empty input (400), missing required fields (422 with issue paths), ambiguous fields (422), unknown symbol (422), unknown plan ID (404)
- `uv run pytest`: 216 passed

## What 7G Does NOT Include

No changes to domain logic, approval workflow, rule evaluation, order intents, positions, fills, or broker integration. Browser UI states were validated via direct API testing; manual browser walkthrough at `http://localhost:5173` confirms the same behavior visually.

## Next Slice

Milestone 7H: Milestone Closeout (tests, documentation, roadmap and status updates, knowledge-base updates).
