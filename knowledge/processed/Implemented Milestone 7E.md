Implemented Milestone 7E.

  Changes:

  - Added TradeCaptureService for backend parse/save/retrieve orchestration.
  - Added FastAPI endpoints:
      - POST /trade-capture/parse
      - POST /trade-capture/save
      - GET /trade-capture/saved/{trade_plan_id}
  - Added API schemas and conversion for drafts, validation issues, and saved summaries.
  - Wired API defaults to local JSON repositories with TRADING_SYSTEM_STORE_PATH support.
  - Added API tests with fake parser and temp JSON stores.
  - Updated README, STATUS, issue map, and added 7E closeout docs.

  Validation:

  - uv run pytest tests\test_api_trade_capture.py tests\test_trade_capture_parser.py tests\test_trade_capture_draft.py: 31 passed
  - uv run pytest tests\test_api_trade_capture.py tests\test_trade_capture_parser.py tests\test_trade_capture_draft.py
    tests\test_reference_lookup_service.py tests\test_api_health.py: 39 passed
  - uv run pytest: 216 passed

  Not committed yet.