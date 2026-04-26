• Decisions are locked: broad list-context, inclusive ISO datetime bounds, and post-import reuse by copying into a new immutable snapshot. I can now write a
  decision-complete implementation plan that keeps provider work deferred and preserves current snapshot audit boundaries.

• Proposed Plan


  # Context Discovery And Attachment Ergonomics

  ## Summary

  Improve Milestone 4 context usability by making stored snapshots searchable from the CLI and adding an explicit copy-to-target workflow for reusing
  existing context with a trade plan, position, or review. Keep snapshots immutable, advisory, local-only, and independent from external providers.

  ## Key Changes

  - Extend MarketContextSnapshotRepository with list_all() in the port, JSON repository, and in-memory repository.
  - Add a read-side MarketContextQueryService.list_snapshots(...) method with optional filters:
      - instrument_id
      - target_type + target_id
      - context_type
      - source
      - inclusive observed_from, observed_to
      - inclusive captured_from, captured_to
  - Update list-context to allow broad discovery:
      - no filters lists all snapshots
      - any combination of filters narrows results
      - target filters still require both --target-type and --target-id
      - output remains sorted by captured_at oldest first
  - Add a copy workflow for existing snapshots:
      - CLI command: copy-context <market-context-snapshot-id> --target-type <trade-plan|position|trade-review> --target-id <uuid>
      - service method loads the original snapshot, validates the target exists, validates the target instrument matches the original snapshot instrument,
        then creates a new MarketContextSnapshot
      - copied snapshot preserves instrument_id, context_type, source, source_ref, observed_at, and payload
      - copied snapshot receives a new id, target link, and new captured_at
      - original snapshot is never mutated

  ## CLI Interface

  - Extend list-context options:
      - --instrument-id <uuid>
      - --target-type <trade-plan|position|trade-review>
      - --target-id <uuid>
      - --context-type <text>
      - --source <text>
      - --observed-from <iso-datetime>
      - --observed-to <iso-datetime>
  - Add copy-context output matching import-style command chaining:
      - context_type
      - source
      - observed_at
      - captured_at
      - target_type
      - target_id

  ## Test Plan

  - Service tests:
      - list_snapshots returns all snapshots when no filters are provided.
      - filters by context_type, source, instrument, target, observed range, and captured range.
      - filters compose correctly.
      - target filter rejects missing target_type or target_id.
      - copy creates a new snapshot and leaves the original unchanged.
      - copy rejects missing source snapshot, unsupported target type, missing target record, and instrument mismatch.
  - CLI tests:
      - list-context works with no filters.
      - list-context --context-type, --source, observed range, and captured range narrow output.
      - list-context still supports instrument and target filters.
      - invalid UUIDs and invalid ISO datetimes produce clear CLI errors.
      - copy-context creates a linked snapshot visible in show-trade-plan, show-position, or show-trade-review.
  - Regression:
      - run focused context/retrieval tests.
      - run full test suite.

  ## Assumptions

  - Existing uncommitted PROJECT.md and STATUS.md changes are user-owned and should be preserved.
  - Docs may be updated only to reflect the new filters and copy workflow.
  - No yfinance, external provider, network fetch, provider ADR, schema migration, or canonical trade mutation is included in this issue.