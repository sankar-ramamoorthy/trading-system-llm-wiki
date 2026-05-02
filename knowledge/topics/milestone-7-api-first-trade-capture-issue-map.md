---
title: Milestone 7 API-First Trade Capture Issue Map
type: topic
status: active
tags: [trading-system, milestone-7, api-first, web-ui, docker, trade-capture, llm-parser]
created: 2026-04-29
updated: 2026-05-02
---

# Milestone 7 API-First Trade Capture Issue Map

Milestone 7 should deliver the first local web product slice for API-first trade capture.

The milestone is not one issue. It should be implemented as a sequence of small issues so each slice has a clear acceptance boundary.

The current application repo state, verified from `README.md`, `STATUS.md`, and `DOCS/` on 2026-05-02, is:

- 7A Dockerized Runtime Foundation is complete.
- 7B Reference Lookup Foundation is complete.
- 7C Trade Capture Draft Contract is complete.
- 7D Natural-Language Parser Boundary is complete.
- 7E FastAPI Trade Capture Service is complete.
- 7F React/Vite Trade Capture Workspace is complete.
- 7G End-to-End Save Workflow is the next planned slice.
- Milestone closeout remains later 7.x work.

## Issue Map

### 7A: Dockerized Runtime Foundation

Create the local runtime shell for the web product.

This includes:

- Docker Compose for backend and frontend runtime surfaces
- FastAPI health endpoint
- Vite React TypeScript frontend shell
- frontend-to-backend health check
- host Ollama configuration placeholders for later parser work

Status: complete in the application repo on 2026-04-29.

Ollama itself should run on the host for now, not as a Compose service.

7A does not implement trade capture, parser behavior, lookup, draft schemas, or save workflow.

### 7B: Reference Lookup Foundation

Add user-facing lookup for instruments and playbooks.

The product should use symbols and playbook slugs, not user-entered UUIDs.

Seeded local reference data is enough for the first version. Management screens are not required yet.

Status: complete in the application repo on 2026-04-29.

### 7C: Trade Capture Draft Contract

Define editable draft contracts for:

- `TradeIdea`
- `TradeThesis`
- `TradePlan`

The contracts should represent parsed user-authored content, required save fields, optional fields, and missing or ambiguous field reporting.

Status: complete in the application repo on 2026-05-02.

Required save fields should remain narrow and explicit:

- `TradeIdea.instrument_symbol`
- `TradeIdea.playbook_slug`
- `TradeIdea.purpose`
- `TradeIdea.direction`
- `TradeIdea.horizon`
- `TradeThesis.reasoning`
- `TradePlan.entry_criteria`
- `TradePlan.invalidation`

Optional fields should remain editable but non-blocking:

- supporting evidence
- risks
- disconfirming signals
- targets
- risk model
- sizing assumptions

Stable issue paths such as `TradeIdea.instrument_symbol` should be used for API/UI error mapping. Reference lookup names matter here, but unresolved reference validation belongs to later save/API work rather than this contract slice.

Implemented 7C behavior:

- editable `TradeIdeaDraft`, `TradeThesisDraft`, and `TradePlanDraft` contracts
- top-level `TradeCaptureDraft` for parsed-but-unsaved trade capture state
- stable field definitions for required and optional draft fields
- missing required field reporting
- ambiguous field issue reporting with preserved candidates
- save-readiness checks that require all required fields and no ambiguity

Validation recorded by the application repo on 2026-05-02:

- `uv run pytest tests\test_trade_capture_draft.py`: 6 passed
- `uv run pytest tests\test_trade_capture_draft.py tests\test_reference_lookup_service.py tests\test_api_health.py`: 14 passed
- `uv run pytest`: 191 passed

7C does not add LiteLLM/Ollama integration, natural-language parsing, persistence, save workflow, frontend capture UI, plan approval, order intent, positions, fills, broker behavior, recommendations, or claim verification.

### 7D: Natural-Language Parser Boundary

Add the LLM-first parser boundary through LiteLLM.

The first target runtime is host Ollama. Later provider routing through LiteLLM can support Groq, OpenAI, or other providers without changing the trade-capture workflow.

The parser must extract only user-authored content. It must not suggest trades, invent levels, verify claims, approve plans, create order intent, open positions, or record fills.

Status: complete in the application repo on 2026-05-02.

Implemented 7D boundary:

- `litellm` dependency
- `TradeCaptureParser.parse(source_text: str) -> TradeCaptureDraft` port
- `TradeCaptureParseError` for parser failures
- deterministic `FakeTradeCaptureParser` for tests and later API wiring
- LiteLLM-backed parser adapter using host Ollama as the first local runtime
- environment-based model and API-base configuration
- strict extraction prompt
- JSON response validation before mapping into the 7C `TradeCaptureDraft`
- 7C draft validation after mapping so missing fields are surfaced rather than guessed

Default local runtime:

```text
TRADING_SYSTEM_LLM_MODEL=ollama_chat/llama3.1
TRADING_SYSTEM_LLM_API_BASE=http://localhost:11434
```

Docker runtime should continue using the host Ollama mapping introduced by 7A:

```text
http://host.docker.internal:11434
```

Implemented 7D failure behavior:

- empty or whitespace-only source text fails before provider call
- missing model or API-base configuration fails before provider call
- provider errors are wrapped as `TradeCaptureParseError`
- invalid JSON, malformed schema, or non-object output fails clearly
- unknown or omitted fields map to `None` or empty lists and are reported through draft validation
- parser-reported ambiguities map to `DraftFieldIssue(issue_type="ambiguous")`
- no parsed result is persisted by 7D

Validation recorded by the application repo on 2026-05-02:

- `uv run pytest tests\test_trade_capture_draft.py tests\test_trade_capture_parser.py`: 22 passed
- `uv run pytest tests\test_trade_capture_draft.py tests\test_trade_capture_parser.py tests\test_reference_lookup_service.py tests\test_api_health.py`: 30 passed
- `uv run pytest`: 207 passed

7D does not add FastAPI trade-capture endpoints, frontend capture UI, save workflow, approval, execution, positions, fills, broker behavior, recommendations, claim verification, or captured LLM output examples.

External documentation checked on 2026-05-02:

- LiteLLM Python SDK `completion()` and Ollama examples
- LiteLLM Ollama provider docs, including `ollama_chat` and JSON mode examples
- LiteLLM structured output / JSON mode docs

### 7E: FastAPI Trade Capture Service

Expose the trade-capture workflow through FastAPI endpoints.

The API should call existing application services and repositories rather than shelling out to the CLI.

Status: complete in the application repo on 2026-05-02.

Implemented 7E boundary:

- trade-capture API schemas for editable drafts, field issues, parse responses, save responses, and saved-result summaries
- `TradeCaptureService` for API orchestration
- `POST /trade-capture/parse`
- `POST /trade-capture/save`
- `GET /trade-capture/saved/{trade_plan_id}`
- configured local JSON store wiring through `TRADING_SYSTEM_STORE_PATH`
- test injection support for fake parsers and temporary repositories

Implemented 7E API behavior:

- parse accepts raw source text and returns editable draft data, validation issues, and `ready_to_save`
- parser errors return clear `400` responses
- parse does not persist anything
- save accepts a confirmed editable draft, not raw text only
- save rejects missing or ambiguous required draft fields with `422` and stable issue paths
- save rejects unknown instrument symbols or playbook slugs with `422`
- save creates linked `TradeIdea`, `TradeThesis`, and `TradePlan` records through existing services
- saved plans remain in draft approval state
- saved-result retrieval returns a compact trade-capture summary by `trade_plan_id`

Validation recorded by the application repo on 2026-05-02:

- `uv run pytest tests\test_api_trade_capture.py tests\test_trade_capture_parser.py tests\test_trade_capture_draft.py`: 31 passed
- `uv run pytest tests\test_api_trade_capture.py tests\test_trade_capture_parser.py tests\test_trade_capture_draft.py tests\test_reference_lookup_service.py tests\test_api_health.py`: 39 passed
- `uv run pytest`: 216 passed

7E does not build the React workspace, claim the full browser-backed user-facing end-to-end workflow, approve plans, evaluate rules, create order intents, open positions, record fills, create reviews, add production auth, migrate to Postgres, add broker integration, generate recommendations, or verify claims.

7F remains the React/Vite capture workspace. 7G remains the full UI-backed parse/edit/save acceptance workflow.

Expected endpoint groups:

- health
- reference lookup
- parse trade-capture text
- save confirmed draft
- retrieve saved result summary

### 7F: React/Vite Trade Capture Workspace

Build the focused web workspace.

The UI should include:

- raw trader-language input
- parse action
- editable Idea, Thesis, and Plan sections
- missing or ambiguous field indicators
- explicit save action
- saved result summary

The screen must not expose approval, order intent, position, fill, broker, or recommendation actions.

Status: complete in the application repo on 2026-05-02.

7F replaces the frontend runtime/status shell with the first real trade-capture screen on top of the 7E backend.

7F is frontend-only plus docs, tests, and build validation. It uses existing 7E backend endpoints.

Expected user-facing flow:

1. User pastes normal trader language into a large input.
2. User clicks parse.
3. UI calls `POST /trade-capture/parse`.
4. UI renders editable draft sections:
   - Idea: symbol, playbook, purpose, direction, horizon
   - Thesis: reasoning, evidence, risks, disconfirming signals
   - Plan: entry criteria, invalidation, targets, risk model, sizing assumptions
5. Missing or ambiguous fields are shown clearly next to the relevant section or field.
6. User edits the draft directly in the browser.
7. User explicitly clicks save.
8. UI calls `POST /trade-capture/save`.
9. UI shows a saved-result summary with generated idea, thesis, and plan IDs.

Implemented 7F behavior:

- React/Vite trade-capture workspace as the first screen
- API health and reference count status strip
- raw trader-language input
- parse action using `POST /trade-capture/parse`
- editable `TradeIdea`, `TradeThesis`, and `TradePlan` sections
- field-level missing and ambiguous issue display using stable draft paths
- explicit save action using `POST /trade-capture/save`
- saved-result summary with generated idea, thesis, and plan IDs
- responsive desktop and mobile layout

Validation recorded by the application repo on 2026-05-02:

- `npm.cmd run build`: passed
- `uv run pytest tests\test_api_trade_capture.py tests\test_api_health.py`: 13 passed
- `uv run pytest`: 216 passed

7F does not add plan approval, rule evaluation, order intent creation, position opening, fill recording, broker actions, trade recommendations, claim verification, API key vault behavior, production auth, cloud deployment, or Postgres migration.

7G remains useful as the stricter end-to-end acceptance slice: Docker/browser/manual validation, persistence verification, and final workflow polish over the 7E backend and 7F UI.

Required 7F UI states:

- initial empty draft
- parsing
- parsed with issues
- parsed and ready to save
- saving
- saved
- parse or save error
- API unreachable

The 7F implementation note also records a knowledge-base side effect: the local API-key vault discussion was captured as a raw brainstorm note. That key-vault work remains separate from 7F implementation scope.

### 7G: End-to-End Save Workflow

Wire parse, edit, and save all the way through local JSON persistence.

Saving should create linked `TradeIdea`, `TradeThesis`, and `TradePlan` records only after explicit user confirmation.

Status: next planned slice as of 2026-05-02. Proposed plan captured 2026-05-02.

#### Proposed 7G Steps

1. **Docker stack acceptance run** — start `docker compose up` in the application repo and confirm all containers come up healthy.
2. **Browser golden-path walkthrough** — enter raw trader text, click Parse, confirm draft sections populate, edit one or more fields, click Save, confirm the saved-result summary appears with generated IDs.
3. **Persistence verification** — inspect the local JSON store after save and confirm linked TradeIdea, TradeThesis, and TradePlan records are written and retrievable. Optionally call `GET /trade-capture/saved/{trade_plan_id}` to verify API round-trip.
4. **Edge-case / error state validation** — walk through all known 7F UI states: empty draft, parsing in progress, parsed with issues, parsed and ready to save, save in progress, saved, parse or save error, API unreachable.
5. **Final workflow polish (if needed)** — fix rough edges surfaced during manual testing; no scope expansion.
6. **Record validation** — update application repo STATUS.md and this knowledge base with commands run, pass/fail results, any issues found and resolved, and confirmation of 7G acceptance criteria.

#### 7G Scope Boundaries

7G does not add: plan approval, rule evaluation, order intent creation, position opening or fill recording, broker integration, recommendations or claim verification, API key vault behavior, production auth, cloud deployment, or Postgres migration.

#### 7G Verification Criteria

Done when:

- `docker compose up` starts cleanly
- Browser golden-path walkthrough completes without errors
- Local JSON store contains the saved TradeIdea, TradeThesis, and TradePlan records
- All known UI error states render correctly
- Full test suite still passes (`uv run pytest`: 216+ passed)
- STATUS.md in application repo updated to reflect 7G complete / 7H next

### 7H: Milestone Closeout

Close the milestone with tests, documentation, roadmap/status updates, and knowledge-base updates.

Milestone 7 is complete only when the Dockerized web/API workflow can capture normal trader language, parse it into editable drafts, and save linked records without crossing into approval, execution, broker, or recommendation behavior.

## Related Pages

- [[api-first-trade-capture-product-vision]]
- [[product-roadmap-and-learning-boundaries]]
- [[trade-lifecycle-and-objects]]
- [[application-implementation-status]]
