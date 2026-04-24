• Implemented Issue 10 retrieval workflows on top of the JSON persistence work.

  Changes made:

  - Extended repository ports in /abs/path/C:/Users/bosto/dockerstuff/trading-system/src/trading_system/ports/
    repositories.py with read methods for positions, fills, and lifecycle events.
  - Implemented those read methods for in-memory and JSON repositories.
  - Added /abs/path/C:/Users/bosto/dockerstuff/trading-system/src/trading_system/services/position_query_service.py for
    read-only position detail/timeline workflows.
  - Added CLI commands in /abs/path/C:/Users/bosto/dockerstuff/trading-system/src/trading_system/app/cli.py:
      - uv run trading-system list-positions
      - uv run trading-system list-positions --state closed
      - uv run trading-system show-position <position-id>
      - uv run trading-system show-position-timeline <position-id>
  - Added tests:
      - /abs/path/C:/Users/bosto/dockerstuff/trading-system/tests/test_position_query_service.py
      - /abs/path/C:/Users/bosto/dockerstuff/trading-system/tests/test_cli_retrieval.py
      - expanded /abs/path/C:/Users/bosto/dockerstuff/trading-system/tests/test_json_persistence.py
  - Updated README and Milestone 2 docs with the new commands.

  Validation:

  - uv run pytest passed: 44 passed
  - uv run trading-system demo-planned-trade passed
  - uv run trading-system list-positions passed
  - uv run trading-system list-positions --state closed passed
  - uv run trading-system show-position 96168b20-4881-41fe-8095-2a4a4098175d passed
  - uv run trading-system show-position-timeline 96168b20-4881-41fe-8095-2a4a4098175d passed

  Note: the local ignored .trading-system/store.json now has accumulated demo records from the smoke tests.