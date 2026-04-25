Implemented explicit order-intent cancellation as a narrow follow-on.

  The change adds OrderIntentStatus.CANCELED, a new CancelOrderIntentService, repository update() support in both memory and JSON stores, and a cancel-
  order-intent <order-intent-id> CLI command in /abs/path/C:/Users/bosto/dockerstuff/trading-system/src/trading_system/app/cli.py. Cancellation now persists
  by replacing the frozen OrderIntent record, emits ORDER_INTENT_CANCELED, and FillService.record_manual_fill() rejects canceled order intents before
  recording a fill.

  I also updated the README command/docs and added coverage across workflow, persistence, retrieval, and CLI tests, including canceled status visibility in
  existing show-trade-plan and show-position views. The new service lives in /abs/path/C:/Users/bosto/dockerstuff/trading-system/src/trading_system/
  services/cancel_order_intent_service.py.

  Validation: uv run pytest passed with 104 passed.

  Note: the worktree already had unrelated existing modifications in files such as /abs/path/C:/Users/bosto/dockerstuff/trading-system/src/trading_system/
  services/position_query_service.py and a few other service/test files that I did not edit for this task.