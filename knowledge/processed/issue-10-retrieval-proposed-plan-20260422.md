Proposed Plan


  # Issue 10: Retrieval Workflows For Persisted Positions

  ## Summary

  Add the first read-only workflows on top of Issue 9 JSON persistence so persisted demo data can be inspected without
  opening the JSON file manually. Keep this issue focused on retrieval only: no new domain entities, no OrderIntent,
  no P&L, no create/update CLI commands beyond the existing demo.

  The core user outcomes are:

  - list persisted positions
  - filter positions by lifecycle state
  - show one position with its linked plan, idea, fills, and review
  - show a position’s lifecycle timeline in chronological order

  ## Key Changes

  - Extend repository ports with minimal read methods needed for retrieval:
      - PositionRepository.list_all()
      - FillRepository.list_by_position_id(position_id)
      - LifecycleEventRepository.list_by_entity(entity_type, entity_id)
      - keep TradeReviewRepository.get_by_position_id(position_id) as-is
  - Implement those methods in both JSON and in-memory repositories so existing test patterns remain simple.
  - Add a small read/query service in services/ that composes existing repositories into position detail views without
    putting workflow logic in the CLI.
  - Add read-only CLI commands backed by the same JSON store path used by the demo:
      - uv run trading-system list-positions
      - uv run trading-system list-positions --state open
      - uv run trading-system list-positions --state closed
      - uv run trading-system show-position <position-id>
      - uv run trading-system show-position-timeline <position-id>
  - CLI output should be plain, compact text tables/sections using existing domain fields; no rich table dependency.
  - Invalid UUIDs or missing positions should produce clear Typer errors and non-zero exits.

  ## Behavior Details

  - list-positions shows one row per position:
      - position id
      - state
      - purpose
      - current quantity
      - opened_at
      - closed_at
  - show-position shows:
      - position summary
      - linked trade plan id, approval state, entry criteria, invalidation, risk model
      - linked trade idea id, purpose, direction, horizon, instrument id
      - fills for the position ordered by filled_at
      - review summary if a review exists
  - show-position-timeline shows lifecycle events for the position ordered by occurred_at.
  - If the JSON store does not exist yet, list commands return an empty result instead of failing.
  - No destructive or mutating commands are added.

  ## Test Plan

  - Add service-level tests using in-memory repositories:
      - list all positions
      - filter open and closed positions
      - timeline events are ordered by occurred_at
      - list_by_position_id() returns only matching fills
      - list_by_entity() returns only matching lifecycle events
  - Add CLI tests with tmp_path and TRADING_SYSTEM_STORE_PATH:
      - run/demo-seeded store can be listed
      - list-positions --state closed shows closed positions
      - show-position <id> includes fills and review
      - show-position-timeline <id> shows lifecycle events in order
      - invalid UUID/missing position exits with a clear error
  - Run:
      - uv run pytest
      - uv run trading-system demo-planned-trade
      - uv run trading-system list-positions
      - uv run trading-system show-position <position-id> using an ID from the demo output

  ## Assumptions

  - Issue 10 builds directly on the uncommitted Issue 9 JSON persistence changes currently in the app repo.
  - The JSON store remains the only active durable backend for this issue.
  - Retrieval is intentionally read-only; practical create/update CLI commands are deferred.
  - OrderIntent remains the next domain modeling candidate after persisted data is inspectable.
