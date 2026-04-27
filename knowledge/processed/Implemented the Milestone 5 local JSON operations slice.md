
• Implemented the Milestone 5 local JSON operations slice.

  Changes made:

  - Added JSON store helpers in src/trading_system/infrastructure/json/repositories.py:
      - validate_json_store
      - backup_json_store
      - restore_json_store
  - Added CLI commands in src/trading_system/app/cli.py:
      - validate-store
      - backup-store
      - restore-store <backup-path> --overwrite
  - Added persistence and CLI coverage in:
      - tests/test_json_persistence.py
      - tests/test_cli_workflow_commands.py
  - Updated README, STATUS, and the Milestone 5 design doc.

  Validation:

  - uv run pytest tests\test_json_persistence.py tests\test_cli_workflow_commands.py tests\test_cli_retrieval.py passed: 67 tests
  - uv run pytest passed: 156 tests

  Note: the first sandboxed pytest run failed because uv could not access its cache; rerunning with approved elevated access passed.
