 Implemented Milestone 7C in the app repo.

  Changes:

  - Added/kept the draft contract in C:/Users/bosto/dockerstuff/trading-system/src/trading_system/services/
    trade_capture_draft.py: editable TradeIdeaDraft, TradeThesisDraft, TradePlanDraft, required/optional field
    definitions, stable issue paths, missing/ambiguous issue reporting, and save-readiness checks.
  - Added focused coverage in C:/Users/bosto/dockerstuff/trading-system/tests/test_trade_capture_draft.py.
  - Added the 7C closeout doc and updated the issue map, README, and STATUS.

  Validation:

  - uv run pytest tests\test_trade_capture_draft.py: 6 passed
  - uv run pytest tests\test_trade_capture_draft.py tests\test_reference_lookup_service.py tests\test_api_health.py: 14
    passed
  - uv run pytest: 191 passed