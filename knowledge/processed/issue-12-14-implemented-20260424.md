 Implemented the Issue 12-14 sequence in-place.

  The main changes are in /C:/Users/bosto/dockerstuff/trading-system/src/trading_system/app/cli.py, /C:/Users/bosto/dockerstuff/trading-system/src/
  trading_system/services/position_query_service.py, and /C:/Users/bosto/dockerstuff/trading-system/src/trading_system/services/trade_query_service.py.
  Closed positions now expose read-side realized_pnl, show-position prints it, list-positions includes a realized P&L column, and the CLI now has explicit
  write commands for the core workflow plus list-trade-ideas, list-trade-plans, and show-trade-plan. I also added the minimal repo list methods needed to
  support those read commands and updated README examples in /C:/Users/bosto/dockerstuff/trading-system/README.md.

  Coverage was added in /C:/Users/bosto/dockerstuff/trading-system/tests/test_position_query_service.py, /C:/Users/bosto/dockerstuff/trading-system/tests/
  test_cli_retrieval.py, and /C:/Users/bosto/dockerstuff/trading-system/tests/test_cli_workflow_commands.py for realized P&L, explicit CLI write flow, UUID
  validation, service-layer error surfacing, and record-fill with and without order_intent_id.

  Verification: uv run pytest passed with 59 passed.

  Note: the worktree already had unrelated existing changes and untracked files outside this slice, including src/trading_system/domain/trading/__init__.py,
  src/trading_system/domain/trading/fill.py, src/trading_system/services/fill_service.py, tests/test_json_persistence.py, src/trading_system/domain/trading/
  order_intent.py, src/trading_system/services/create_order_intent_service.py, and tests/test_order_intent_workflow.py. I left those as-is.
