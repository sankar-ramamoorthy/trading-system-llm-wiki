  # Issue 17: Milestone 3 Usability Bundle

  ## Summary

  Implement the next Milestone 3 issue as a broader manual-workflow usability bundle focused on three things:

  - add still-missing thesis inspection commands
  - add narrow, high-value filtering and sorting on existing list commands
  - finish README/examples so documented CLI usage matches the implemented surface

  Do not include OrderIntent cancellation in this issue. Treat cancellation as a separate follow-on Milestone 3 issue if real usage still shows that gap
  after this bundle lands.

  ## Key Changes

  ### CLI inspection additions

  Add thesis inspection as the remaining high-value missing read surface:

  - list-trade-theses
  - show-trade-thesis <trade-thesis-id>

  Shape the thesis read model around chaining and inspection, not editing.

  list-trade-theses should show one header row and deterministic rows with:

  - TRADE_THESIS_ID
  - TRADE_IDEA_ID
  - PURPOSE
  - DIRECTION
  - PLAN_COUNT
  - TRADE_IDEA_CREATED_AT

  Use the linked trade idea timestamp as the list ordering/sorting basis because TradeThesis currently has no timestamp and this issue should not add one.

  show-trade-thesis should render sections in this order:

  1. top-level thesis fields
  2. Trade idea
  3. Trade plans

  Use the same field_name: value formatting and blank-line spacing already established by Issue 16.

  ### Filtering and sorting

  Add narrow filters only where they materially help daily CLI use. Keep all matching exact against persisted values, and combine multiple filters with
  logical AND.

  Add explicit --sort to list commands using oldest and newest. Defaults remain the current stable chronological order unless the command is newly added.

  Planned CLI surface:

  - list-trade-ideas
      - add --purpose
      - add --direction
      - add --status
      - add --sort oldest|newest
  - list-trade-theses
      - add --purpose
      - add --direction
      - add --has-plan
      - add --sort oldest|newest
      - sort by linked trade idea creation time
  - list-trade-plans
      - add --approval-state
      - add --sort oldest|newest
  - list-trade-reviews
      - add --rating
      - add --purpose
      - add --direction
      - add --sort oldest|newest
  - list-positions
      - keep --state
      - add --purpose
      - add --has-review/--no-review
      - add --sort oldest|newest

  Keep all current command names and existing output formats. This issue is still usability-only, not a domain-behavior change.

  ### Read/query layer support

  Extend the read layer rather than pushing filtering into the CLI loop ad hoc.

  Add thesis-oriented read models and query methods in the existing trade query service area:

  - thesis list item with linked idea context and plan count
  - thesis detail with linked trade idea and linked trade plans
  - service methods for thesis listing/detail plus filter/sort support for ideas, theses, and plans

  Add list_all() to TradeThesisRepository and implement it in memory and JSON persistence so thesis listing does not rely on persistence internals.

  For reviews and positions, keep current query-service responsibilities and add filter/sort parameters there rather than changing domain services.

  ### Documentation and Milestone decision

  Update README examples and current-capabilities text so the documented CLI matches the implemented commands, flags, and read-side behavior after this
  issue.

  Document the Milestone 3 decision on cancellation clearly:

  - no cancel-order-intent command in this issue
  - no new OrderIntentStatus values in this issue
  - cancellation remains a separate Milestone 3 follow-on candidate, not part of this usability bundle

  - thesis repository support:
      - TradeThesisRepository.list_all() in memory and JSON
  - trade query service:
      - thesis list ordering by linked idea creation time
      - thesis detail includes linked idea and linked plans
      - thesis filters work individually and together
      - sort modes return deterministic oldest/newest order
  - CLI retrieval:
      - list-trade-theses header, row shape, empty state, and sort behavior
      - show-trade-thesis section order and linked-plan output
      - new filters on ideas, plans, reviews, and positions
      - --has-review/--no-review behavior on positions
      - invalid UUID and missing-record behavior unchanged for all show-* commands
  - docs-alignment coverage:
      - at minimum, update workflow tests or retrieval tests so the newly documented command/flag combinations are exercised

  Minimum verification:

  - targeted retrieval/query tests for thesis and filters
  - tests/test_cli_retrieval.py
  - any affected query-service tests
  - full uv run pytest

  ## Assumptions And Defaults

  - Use one broader Issue 17 usability bundle rather than splitting filters, thesis inspection, and docs into separate immediate issues.
  - Thesis inspection is the only new read-command addition in this issue.
  - Filter matching is exact and conjunctive; no fuzzy search, substring search, or date-range filtering.
  - Sort choices are only oldest and newest.
  - list-trade-theses uses linked trade idea creation time for ordering/sorting because thesis timestamps do not exist and should not be introduced here.
  - OrderIntent cancellation belongs, at most, in a later separate Milestone 3 issue after this bundle, not in this plan.