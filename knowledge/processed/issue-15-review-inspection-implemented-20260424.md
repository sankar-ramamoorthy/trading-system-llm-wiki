• Implemented the review inspection slice. The CLI now supports list-trade-reviews and show-trade-review, backed by a new read-only ReviewQueryService in /
  C:/Users/bosto/dockerstuff/trading-system/src/trading_system/services/review_query_service.py and extended review repository methods in /C:/Users/bosto/
  dockerstuff/trading-system/src/trading_system/ports/repositories.py. I also wired the commands into /C:/Users/bosto/dockerstuff/trading-system/src/
  trading_system/app/cli.py and added repository support for TradeReview.get() and list_all() in the JSON and in-memory implementations.

  I added coverage for the new service and CLI in /C:/Users/bosto/dockerstuff/trading-system/tests/test_review_query_service.py, /C:/Users/bosto/
  dockerstuff/trading-system/tests/test_cli_retrieval.py, and /C:/Users/bosto/dockerstuff/trading-system/tests/test_json_persistence.py. While validating,
  an existing lifecycle-event ordering assumption surfaced in JSON persistence, so I fixed repository-level lifecycle event ordering in both memory and JSON
  repositories to keep persisted reads stable.

  Verification: uv run pytest passed with 67 passed.
