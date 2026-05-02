 Proposed Plan


  # Milestone 7E Plan: FastAPI Trade Capture Service

  ## Summary

  Implement the backend API surface for trade capture. 7E should expose parse, save, and saved-result retrieval through FastAPI over the existing parser,
  reference lookup, planning service, and JSON repositories. It should not build the React workspace or claim the full user-facing end-to-end workflow;
  7F/7G remain for UI wiring and Docker/manual acceptance.

  ## Key Changes

  - Add trade-capture API schemas for draft sections, field issues, parse request/response, save request/response, and saved-result summary.
  - Add a backend trade-capture service that:
      - parses raw text through TradeCaptureParser
      - validates draft readiness through the 7C contract
      - resolves instrument_symbol and playbook_slug through ReferenceLookupService
      - saves confirmed drafts through TradePlanningService
      - retrieves a saved result by trade_plan_id through TradeQueryService
  - Add FastAPI endpoints:
      - POST /trade-capture/parse
      - POST /trade-capture/save
      - GET /trade-capture/saved/{trade_plan_id}
  - Keep existing reference endpoints unchanged.
  - Wire API defaults to local JSON repositories from TRADING_SYSTEM_STORE_PATH, falling back to .trading-system/store.json.
  - Allow tests to inject fake parser and temporary repositories into create_app(...).

  ## API Behavior

  - POST /trade-capture/parse
      - Input: { "source_text": "..." }
      - Output: editable draft, validation issues, and ready_to_save.
      - Parser errors return a clear 400 response.
      - No persistence occurs.
  - POST /trade-capture/save
      - Input: confirmed editable draft, not raw text only.
      - Rejects missing/ambiguous required draft fields with 422 and stable issue paths.
      - Rejects unknown instrument symbols or playbook slugs with 422.
      - Creates linked TradeIdea, TradeThesis, and TradePlan through existing services.
      - Returns generated IDs plus a compact saved summary.
      - Does not approve the plan, evaluate rules, create order intent, open positions, record fills, or create reviews.
  - GET /trade-capture/saved/{trade_plan_id}
      - Downstream objects such as order intents, positions, fills, reviews, and market context are not added to this response.

  - API parse tests with FakeTradeCaptureParser:
      - valid parse returns draft and readiness flags
      - missing fields surface issue paths
      - parser failure returns 400
      - parse does not write JSON records
  - API save tests using a temp JSON store:
      - complete draft creates linked TradeIdea, TradeThesis, and TradePlan
      - saved result can be retrieved by trade_plan_id
      - missing required fields reject with 422
      - ambiguous draft rejects with 422
      - unknown symbol/playbook rejects with 422
      - saved plan remains approval_state = "draft"
  - Regression:
      - uv run pytest tests\test_api_trade_capture.py tests\test_trade_capture_parser.py tests\test_trade_capture_draft.py
      - uv run pytest tests\test_reference_lookup_service.py tests\test_api_health.py
      - uv run pytest

  ## Documentation

  - Add DOCS/milestone-7e-fastapi-trade-capture-service.md.
  - Mark 7E complete in DOCS/milestone-7-issue-map.md after validation.
  - Update README and STATUS with the new trade-capture API endpoints and boundary notes.
  - Record that 7F is next for the React/Vite capture workspace, and 7G remains the full UI-backed parse/edit/save acceptance workflow.

  ## Assumptions

  - 7E owns backend parse/save/retrieve endpoints.
  - 7G remains meaningful as the user-facing end-to-end workflow: React UI plus Docker/manual acceptance over the 7E backend.
  - No production auth, cloud deployment, Postgres migration, broker integration, approval/execution action, generated recommendation, or claim
    verification is included.
