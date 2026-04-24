• Proposed Plan


  # Next Milestone Set: Milestones 3-5

  ## Summary

  Plan the next three milestones as a practical, local-first roadmap:

  1. Milestone 3: Manual Workflow Usability
  2. Milestone 4: Read-Only Market Context
  3. Milestone 5: Review, Learning, and Local Operations

  This sequence fits the current repo state best. Milestone 2 already covers the core lifecycle, JSON persistence, retrieval, narrow OrderIntent, and
  basic realized P&L. The next set should make the system more useful for repeated manual trading before taking on database migration, broker adapters, or
  RL work.

  The roadmap should explicitly defer:

  - Postgres/Alembic as the active persistence path
  - broker integration
  - live market execution
  - FastAPI
  - RL / simulation milestones
  - portfolio analytics platforms

  ## Milestone 3: Manual Workflow Usability

  Make the existing local CLI workflow comfortable and complete for repeated manual use.

  Key changes:

  - Add the missing high-value read/write workflow polish around the current entities rather than introducing new major domain objects.
  - Add practical inspection commands for thesis and review records if needed for chaining:
      - list-trade-reviews
      - show-trade-review
      - optionally show-trade-thesis if thesis inspection is blocking manual usage
  - Add narrow correction/amendment workflows only for records that are currently painful to re-enter:
      - cancel-order-intent or equivalent narrow terminal state change
      - fill correction should not be added yet unless the design is made audit-safe first
  - Improve output consistency across all commands:
      - stable field order
      - consistent ids in outputs
      - consistent empty-state messages
      - consistent date/time formatting
  - Extend read-side views to expose the most useful trading facts already present in storage:
      - position realized P&L in list and detail output
      - rule evaluation summary in plan detail
      - review metadata in position and review lists
  - Add lightweight local filtering/sorting where it helps daily use:
      - list-positions --state
      - optional filters like --purpose, --approval-state, --has-review
      - sort by created/opened/reviewed timestamps in a stable default order

  Public interface additions:

  - New CLI read commands for review inspection
  - Optional CLI filters on existing list commands
  - Narrow order-intent status transition if cancellation is included

  Acceptance for Milestone 3:

  - A user can create, inspect, execute, and review trades repeatedly without relying on the demo command.
  - The CLI exposes enough read-side information to chain commands without opening the JSON file manually.
  - No new persistence backend is introduced.

  ## Milestone 4: Read-Only Market Context

  Add external context on the read side only, without changing the system’s source-of-truth boundaries.

  Key changes:

  - Introduce a small context layer for market snapshots attached to planning or review workflows, not a live trading engine.
  - Scope context to read-only, pull-based snapshots:
      - recent price bars
      - simple volatility/range context
      - optional instrument metadata if needed
  - Keep context separate from trade meaning:
      - TradeIdea, TradeThesis, TradePlan, Position, and Fill remain canonical
      - market data is an input for planning/review, not a replacement for domain state
  - Store only the minimum local artifacts needed for reproducibility:
      - snapshot timestamp
      - provider/source label
      - fetched values used in the view
  - Add CLI commands oriented around manual planning and review:
      - show-instrument-context <instrument-id or symbol>
      - show-trade-plan-context <trade-plan-id>
      - optional snapshot capture command if explicit persistence is needed
  - Keep provider wiring abstract enough that the repo can start with a stub/local file/sample adapter before any real integration commitment

  Public interface additions:

  - New read-only context query commands
  - New local context snapshot/read model types
  - New repository/adapter boundary for market/context reads

  Acceptance for Milestone 4:

  - A user can inspect current or recent market context while preparing or reviewing a trade.
  - Context remains clearly separated from domain records and execution facts.
  - No broker orders, no automated reactions, and no live streaming are introduced.

  ## Milestone 5: Review, Learning, and Local Operations

  Deepen the journaling and learning loop while keeping the app local-first.

  Key changes:

  - Expand TradeReview usability before adding any AI or RL:
      - tags
      - setup/category labels
      - review outcome markers
      - searchable lessons/follow-up actions
  - Add review-centric read models and CLI flows:
      - list-trade-reviews
      - filtering by rating/tag/outcome/date range
      - review summaries by period
  - Add narrow local reporting/export features that fit the current architecture:
      - export reviews/positions to CSV or JSON
      - local summary report command over closed positions
  - Add local operational hardening for the JSON-first phase:
      - backup/export command for the store
      - import/restore only if it can be done safely and audibly
      - corruption/error messaging improvements around local persistence
  - Keep analytics intentionally narrow:
      - counts, realized P&L totals, average rating, tagged-review summaries
      - no portfolio engine, no tax-lot accounting, no performance attribution stack

  - Local backup/export commands for JSON-backed operation

  Acceptance for Milestone 5:

  - A user can use the system not only to record trades, but to extract structured learning from them.
  - Local persistence is still the active operating mode, but it is more robust and easier to manage.
  - Reporting remains trade-journal-grade, not portfolio-platform-grade.

  ## Test Plan

  Cross-milestone validation should include:

  - end-to-end CLI workflows that create, inspect, execute, and review trades without demo-planned-trade
  - stable list/show output behavior with empty data, single-record data, and multi-record data
  - read-only context commands with missing/unavailable provider data
  - review filtering and reporting over mixed review sets
  - export/backup commands producing deterministic, parseable output
  - regression coverage for current invariants:
      - Position still originates from TradePlan
      - OrderIntent remains distinct from Fill
      - realized P&L stays read-side only
      - context data does not become canonical trade state

  ## Assumptions And Defaults

  - Use a three-milestone horizon only.
  - Favor practical trading-system work, not RL-first planning.
  - Keep the system local-first across these three milestones; do not make Postgres/Alembic the active path yet.
  - Treat the Milestone 3 RL note in the knowledge base as exploratory only and out of scope for this milestone set.
  - Prefer CLI and read-model improvements over introducing new architectural layers.
  - If a correction/edit workflow cannot be made clearly auditable, defer it rather than adding mutable behavior casually.