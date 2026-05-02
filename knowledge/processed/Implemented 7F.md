 Implemented 7F.

  Application repo changes:

  - Replaced the frontend status shell with the React/Vite trade-capture workspace.
  - Added parse/edit/save flow against the 7E API.
  - Added field-level missing/ambiguous issue display.
  - Added saved-result summary with generated idea/thesis/plan IDs.
  - Updated README, STATUS, issue map, and added DOCS/milestone-7f-react-trade-capture-workspace.md.

  Knowledge-base change:

  - Added raw key-vault brainstorm note at knowledge/raw/brainstorm-20260502-local-api-key-vault.md.

  Validation:

  - npm.cmd run build: passed
  - uv run pytest tests\test_api_trade_capture.py tests\test_api_health.py: 13 passed
  - uv run pytest: 216 passed