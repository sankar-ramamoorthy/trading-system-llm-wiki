---
title: Implemented Milestone 7E
type: processed-note
status: processed
tags: [trading-system, milestone-7, fastapi, trade-capture, api, draft-workflow]
created: 2026-05-02
---

# Implemented Milestone 7E

The application repo has implemented Milestone 7E: FastAPI Trade Capture Service.

The raw note says the application repo changes were not committed yet when captured. The implementation was still verified from the working application repo docs, status, source, and tests.

## Implemented Scope

Milestone 7E exposes backend trade-capture workflows through FastAPI over the parser, reference lookup, planning service, query service, and local JSON repositories.

Implemented surface:

- `TradeCaptureService`
- trade-capture API schemas for drafts, validation issues, and saved summaries
- `POST /trade-capture/parse`
- `POST /trade-capture/save`
- `GET /trade-capture/saved/{trade_plan_id}`
- local JSON repository wiring through `TRADING_SYSTEM_STORE_PATH`
- test injection support for fake parsers and temporary repositories

## Endpoint Behavior

`POST /trade-capture/parse` accepts raw source text, parses through the configured parser, returns editable draft data, returns validation issues and `ready_to_save`, returns parser errors as clear `400` responses, and persists nothing.

`POST /trade-capture/save` accepts a confirmed editable draft, rejects missing required fields with `422`, rejects ambiguous fields with `422`, rejects unknown instrument symbols or playbook slugs with `422`, resolves references through lookup, creates linked `TradeIdea`, `TradeThesis`, and `TradePlan` records, and leaves saved plans in draft approval state.

`GET /trade-capture/saved/{trade_plan_id}` returns a compact saved trade-capture summary by trade plan id. It does not include downstream lifecycle objects such as order intents, positions, fills, reviews, or market context.

## Boundaries

Milestone 7E does not add React/Vite capture workspace, full browser-backed parse/edit/save acceptance workflow, plan approval, rule evaluation, order intent creation, position opening, fill recording, review creation, broker integration, generated recommendations, thesis claim verification, production auth, cloud deployment, or Postgres migration.

## Validation

Recorded by the application repo on 2026-05-02:

```text
uv run pytest tests\test_api_trade_capture.py tests\test_trade_capture_parser.py tests\test_trade_capture_draft.py: 31 passed
uv run pytest tests\test_api_trade_capture.py tests\test_trade_capture_parser.py tests\test_trade_capture_draft.py tests\test_reference_lookup_service.py tests\test_api_health.py: 39 passed
uv run pytest: 216 passed
```

## Next

Milestone 7F is the next planned slice: React/Vite Trade Capture Workspace.

## Related Pages

- [[milestone-7-api-first-trade-capture-issue-map]]
- [[implemented-milestone-7d-20260502]]
- [[proposed-milestone-7e-fastapi-trade-capture-service-20260502]]
- [[api-first-trade-capture-product-vision]]
- [[application-implementation-status]]
