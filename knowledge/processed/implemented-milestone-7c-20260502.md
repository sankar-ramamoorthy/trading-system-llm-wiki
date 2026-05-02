---
title: Implemented Milestone 7C
type: processed-note
status: processed
tags: [trading-system, milestone-7, api-first, trade-capture, draft-contract]
created: 2026-05-02
---

# Implemented Milestone 7C

The application repo has implemented Milestone 7C: Trade Capture Draft Contract.

## Implemented Scope

The implementation adds the service-layer draft contract in:

```text
C:\Users\bosto\dockerstuff\trading-system\src\trading_system\services\trade_capture_draft.py
```

It defines:

- `TradeIdeaDraft`
- `TradeThesisDraft`
- `TradePlanDraft`
- `TradeCaptureDraft`
- `DraftFieldDefinition`
- `DraftFieldIssue`
- required and optional field-definition helpers

The contract represents parsed-but-unsaved trade capture state. It uses user-facing `instrument_symbol` and `playbook_slug`; internal UUID resolution remains later API/save workflow work.

## Durable Behavior

Required save fields:

- `TradeIdea.instrument_symbol`
- `TradeIdea.playbook_slug`
- `TradeIdea.purpose`
- `TradeIdea.direction`
- `TradeIdea.horizon`
- `TradeThesis.reasoning`
- `TradePlan.entry_criteria`
- `TradePlan.invalidation`

Optional editable fields:

- `TradeThesis.supporting_evidence`
- `TradeThesis.risks`
- `TradeThesis.disconfirming_signals`
- `TradePlan.targets`
- `TradePlan.risk_model`
- `TradePlan.sizing_assumptions`

Missing required fields and ambiguous parser outputs are surfaced as explicit issues with stable paths such as `TradeIdea.instrument_symbol`. Drafts are ready to save only when required fields are present and no ambiguity remains.

## Boundaries

Milestone 7C does not add:

- natural-language parsing
- LiteLLM or Ollama calls
- FastAPI trade-capture endpoints
- local JSON save workflow
- frontend trade-capture workspace
- approval, rule evaluation, order intent, position, fill, broker, recommendation, or claim-verification behavior

## Validation

Recorded by the application repo on 2026-05-02:

```text
uv run pytest tests\test_trade_capture_draft.py: 6 passed
uv run pytest tests\test_trade_capture_draft.py tests\test_reference_lookup_service.py tests\test_api_health.py: 14 passed
uv run pytest: 191 passed
```

## Next

Milestone 7D is the next planned slice: Natural-Language Parser Boundary.

## Related Pages

- [[milestone-7-api-first-trade-capture-issue-map]]
- [[api-first-trade-capture-product-vision]]
- [[application-implementation-status]]
