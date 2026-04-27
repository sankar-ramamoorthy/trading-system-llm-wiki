Proposed Plan
  # Milestone 5 Local JSON Backup And Restore

  ## Summary

  Add a narrow local-ops slice for the active JSON workflow: validate the configured store, create an inspectable timestamped JSON backup, and restore
  from a backup only with explicit overwrite consent. Keep this outside domain logic and avoid new persistence architecture.

  ## Key Changes

  - Add JSON store operations in infrastructure, not domain:
      - validate an existing store file using the same collection-shape checks as JsonStore
      - create backups by copying the exact configured store JSON
      - restore by atomically replacing the configured store from a validated backup file
  - Add CLI commands:
      - backup-store --output-dir <path>
          - defaults to .trading-system/backups
          - creates the output directory if needed
          - fails if the configured store does not exist or is invalid
      - restore-store <backup-path> --overwrite
          - uses atomic replacement through the existing JSON write pattern
          - prints restored_store_path: and backup_path:
      - validate-store
          - validates the configured store path
          - prints store path, collection names, and total record count
          - fails clearly for missing or invalid stores
  - Update README and STATUS with the new local operations commands and the completed slice wording.

  ## Test Plan

  - Add focused tests for JSON store operations:
      - backup copies an existing valid store exactly
      - backup fails for missing store
      - backup fails for invalid JSON
      - restore refuses an existing store without --overwrite
      - restore validates backup before replacement
      - restore replaces the configured store when --overwrite is present
  - Add CLI tests:
      - backup-store creates a timestamped .json backup and reports paths
      - restore-store round-trips a backed-up store into a fresh or overwritten configured store
      - validate-store reports valid collection counts
      - invalid/missing files return non-zero exit codes with clear messages
  - Run:
      - uv run pytest tests\test_json_persistence.py tests\test_cli_workflow_commands.py tests\test_cli_retrieval.py

  ## Assumptions

  - Backup format is an exact JSON copy, not ZIP.
  - Restore requires --overwrite when the configured store already exists.
  - This slice does not add encryption, compression, scheduled backups, cloud sync, migrations, or Postgres backup support.
  - The active source of truth remains TRADING_SYSTEM_STORE_PATH or .trading-system/store.json.
