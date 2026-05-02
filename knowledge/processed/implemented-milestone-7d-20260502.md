---
title: Implemented Milestone 7D
type: processed-note
status: processed
tags: [trading-system, milestone-7, parser-boundary, litellm, ollama, trade-capture]
created: 2026-05-02
---

# Implemented Milestone 7D

The application repo has implemented Milestone 7D: Natural-Language Parser Boundary.

## Implemented Scope

Milestone 7D adds the parser boundary for API-first trade capture without adding API endpoints, frontend capture UI, save workflow, or persistence.

Implemented application repo paths:

```text
C:\Users\bosto\dockerstuff\trading-system\src\trading_system\ports\trade_capture_parser.py
C:\Users\bosto\dockerstuff\trading-system\src\trading_system\services\trade_capture_parser.py
C:\Users\bosto\dockerstuff\trading-system\src\trading_system\infrastructure\litellm\trade_capture_parser.py
C:\Users\bosto\dockerstuff\trading-system\tests\test_trade_capture_parser.py
```

The implementation adds:

- `litellm` dependency
- `TradeCaptureParser` port
- `TradeCaptureParseError`
- deterministic `FakeTradeCaptureParser`
- LiteLLM-backed parser adapter
- `TRADING_SYSTEM_LLM_MODEL` and `TRADING_SYSTEM_LLM_API_BASE` configuration
- strict extraction prompt
- JSON response validation
- mapping into the Milestone 7C `TradeCaptureDraft` contract

## Runtime Boundary

Docker defaults point the API container at host Ollama:

```text
TRADING_SYSTEM_LLM_MODEL=ollama_chat/llama3.1
TRADING_SYSTEM_LLM_API_BASE=http://host.docker.internal:11434
```

Native local runs commonly use:

```text
TRADING_SYSTEM_LLM_API_BASE=http://localhost:11434
```

## Durable Behavior

The parser extracts only user-authored trade-capture content.

It must not:

- suggest trades
- invent missing values
- verify claims
- approve plans
- create order intents
- open positions
- record fills
- persist parsed results

Parser failures raise `TradeCaptureParseError`. Missing fields and ambiguity are surfaced through the existing 7C draft validation contract.

## Boundaries

Milestone 7D does not add:

- FastAPI trade-capture endpoints
- frontend trade-capture workspace
- local JSON save workflow
- persistence from parser output
- approval, rule evaluation, order intent, position, fill, broker, recommendation, or claim-verification behavior

## Validation

Recorded by the application repo on 2026-05-02:

```text
uv run pytest tests\test_trade_capture_draft.py tests\test_trade_capture_parser.py: 22 passed
uv run pytest tests\test_trade_capture_draft.py tests\test_trade_capture_parser.py tests\test_reference_lookup_service.py tests\test_api_health.py: 30 passed
uv run pytest: 207 passed
```

## Next

Milestone 7E is the next planned slice: FastAPI Trade Capture Service.

## Related Pages

- [[milestone-7-api-first-trade-capture-issue-map]]
- [[implemented-milestone-7c-20260502]]
- [[proposed-milestone-7d-natural-language-parser-boundary-20260502]]
- [[api-first-trade-capture-product-vision]]
- [[application-implementation-status]]
