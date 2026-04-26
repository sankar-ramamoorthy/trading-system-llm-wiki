Implemented the context discovery and copy workflow.

  What changed:

  - list-context now supports broad discovery with optional filters:
      - --instrument-id
      - --target-type + --target-id
      - --context-type
      - --source
      - --observed-from/--observed-to
      - --captured-from/--captured-to
  - list-context can now run with no filters and lists all snapshots.
  - Added copy-context <snapshot-id> --target-type ... --target-id ....
  - Copying creates a new immutable linked snapshot and leaves the original unchanged.
  - Added repository list_all() for market context snapshots.
  - Updated README examples.
  - Added service, persistence, and CLI tests.

  Verification:

  - Focused context/read tests: 43 passed
  - Full suite: 129 passed

  Note: PROJECT.md and STATUS.md already had uncommitted edits when this started; I preserved them and did not modify them further.

