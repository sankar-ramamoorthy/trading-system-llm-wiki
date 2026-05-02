---
title: Proposed Milestone 7E FastAPI Trade Capture Service
type: processed-note
status: processed
tags: [trading-system, milestone-7, fastapi, trade-capture, api, draft-workflow]
created: 2026-05-02
---

# Proposed Milestone 7E FastAPI Trade Capture Service

This note processes the raw proposed plan for Milestone 7E.

## Status

At the time this proposal was captured, Milestone 7E was not implemented in the application repo yet. The application repo state then was:

- 7A Dockerized Runtime Foundation: complete
- 7B Reference Lookup Foundation: complete
- 7C Trade Capture Draft Contract: complete
- 7D Natural-Language Parser Boundary: complete
- 7E FastAPI Trade Capture Service: next proposed slice

Supersession note: a later raw implementation note on 2026-05-02 recorded that Milestone 7E was implemented. See [[implemented-milestone-7e-20260502]] for the current state.

## Proposed Scope

7E should implement the backend API surface for trade capture without building the React workspace or claiming the full user-facing end-to-end workflow.

The planned backend surface:

- `POST /trade-capture/parse`
- `POST /trade-capture/save`
- `GET /trade-capture/saved/{trade_plan_id}`

The API should sit over existing boundaries:

- `TradeCaptureParser`
- 7C `TradeCaptureDraft` validation
- `ReferenceLookupService`
- `TradePlanningService`
- `TradeQueryService`
- local JSON repositories

The API must not shell out to the CLI.

## Parse Behavior

`POST /trade-capture/parse` should:

- accept `{ "source_text": "..." }`
- parse through the configured `TradeCaptureParser`
- return editable draft sections
- return validation issues and `ready_to_save`
- return parser failures as clear `400` responses
- persist nothing

## Save Behavior

`POST /trade-capture/save` should:

- accept a confirmed editable draft, not raw text only
- reject missing required fields with `422`
- reject ambiguous draft fields with `422`
- reject unknown instrument symbols or playbook slugs with `422`
- preserve stable issue paths for API/UI mapping
- resolve `instrument_symbol` and `playbook_slug` through reference lookup
- create linked `TradeIdea`, `TradeThesis`, and `TradePlan` records through existing services
- leave the saved plan in draft approval state
- return generated IDs plus a compact saved summary

Save must not approve plans, evaluate rules, create order intents, open positions, record fills, create reviews, or trigger execution behavior.

## Saved Result Retrieval

`GET /trade-capture/saved/{trade_plan_id}` should retrieve a compact saved trade-capture result by `trade_plan_id`.

Downstream lifecycle objects such as order intents, positions, fills, reviews, and market context should not be included in this response.

## Testing Direction

Planned test coverage:

- parse with `FakeTradeCaptureParser`
- missing fields surface stable issue paths
- parser failures return `400`
- parse does not write JSON records
- complete save creates linked `TradeIdea`, `TradeThesis`, and `TradePlan`
- saved result can be retrieved by `trade_plan_id`
- missing/ambiguous fields reject with `422`
- unknown symbol or playbook rejects with `422`
- saved plan remains `approval_state = "draft"`
- existing parser, draft, reference lookup, and health tests still pass
- full suite still passes

## Boundary

7E does not add:

- React/Vite capture workspace
- full UI-backed parse/edit/save acceptance workflow
- production auth
- cloud deployment
- Postgres migration
- broker integration
- approval or execution actions
- generated recommendations
- claim verification

7F remains the React/Vite capture workspace. 7G remains the full UI-backed end-to-end workflow over the 7E backend.

## Related Pages

- [[milestone-7-api-first-trade-capture-issue-map]]
- [[implemented-milestone-7d-20260502]]
- [[api-first-trade-capture-product-vision]]
- [[application-implementation-status]]
