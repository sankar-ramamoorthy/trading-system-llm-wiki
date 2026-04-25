 Implemented Issue 17 across the read side, CLI, and docs.

  The main changes are in /C:/Users/bosto/dockerstuff/trading-system/src/trading_system/app/cli.py, /C:/Users/bosto/dockerstuff/trading-system/src/
  trading_system/services/trade_query_service.py, /C:/Users/bosto/dockerstuff/trading-system/src/trading_system/services/review_query_service.py, /C:/Users/
  bosto/dockerstuff/trading-system/src/trading_system/services/position_query_service.py, and the thesis repository interfaces/implementations in /C:/Users/
  bosto/dockerstuff/trading-system/src/trading_system/ports/repositories.py, /C:/Users/bosto/dockerstuff/trading-system/src/trading_system/infrastructure/
  memory/repositories.py, and /C:/Users/bosto/dockerstuff/trading-system/src/trading_system/infrastructure/json/repositories.py. This adds list-trade-theses
  and show-trade-thesis, exact-match filters plus --sort oldest|newest on the planned list commands, and keeps the filtering logic in the query layer
  instead of ad hoc CLI loops.