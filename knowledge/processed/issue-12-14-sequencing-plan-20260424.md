 Proposed Plan


  # Recommended Issue 12-14 Sequence

  ## Summary

  show-position-timeline is already implemented, so it should not be scheduled again as a next issue.

  Recommended next three issues:

  1. Issue 12: Basic Realized P&L for Closed Positions
  2. Issue 13: Core Write CLI Commands
  3. Issue 14: CLI Usability and Read-Side Polish

  This order fits the current repo state on 2026-04-24:

  - retrieval commands already exist
  - timeline already exists
  - domain execution facts are now stable enough for minimal realized P&L
  - CLI still depends on demo-planned-trade for all write workflows
  - practical CLI usage should build on stable read models, not the reverse

  ## Issue 12: Basic Realized P&L for Closed Positions

  ### Summary

  Add minimal realized P&L derived from persisted fills for simple closed positions. Keep it narrow and auditable.

  ### Key Changes

  - Add a small read-side P&L computation service or helper that derives realized P&L from fills for one position.
  - Scope the calculation to the current supported execution model:
      - long-only positions
      - manual fills
      - no commissions
      - no fees
      - no tax lots
      - no partial-lot accounting policy beyond current weighted-average behavior
  - Use current position invariants as the guardrail:
      - only compute realized P&L for positions whose lifecycle state is closed
      - reject or omit P&L for open positions
      - rely on existing fill history ordered by filled_at
  - Recommended formula:
      - total sell proceeds minus total buy cost for the position
      - because current domain already prevents oversell and reversal, this is decision-safe for the current slice
  - Extend the PositionDetail read model to include:
      - realized_pnl: Decimal | None
  - Update show-position to print realized P&L for closed positions and blank or N/A for open positions.
  - Do not add portfolio aggregation, unrealized P&L, win/loss analytics, percentages, fees, or reporting exports.

  ### Public Interface Changes

  - PositionDetail gains realized_pnl: Decimal | None
  - show-position output gains one realized P&L field for closed positions

  ### Test Plan

  - closed position with one buy and one sell computes correct realized P&L
  - closed position with multiple buys and one closing sell computes correct realized P&L from total cost vs total proceeds
  - open position returns realized_pnl=None
  - JSON-backed retrieval shows realized P&L correctly in CLI output
  - existing fill/position behavior remains unchanged

  ## Issue 13: Core Write CLI Commands

  ### Summary

  Move from demo-only write flows to explicit CLI commands for the core lifecycle.

  ### Key Changes

  - Add practical Typer commands for the current narrow workflow:
      - create-trade-idea
      - create-trade-thesis
      - create-trade-plan
      - approve-trade-plan
      - evaluate-trade-plan-rules
      - create-order-intent
      - open-position
      - record-fill
      - create-trade-review
  - Each command should:
      - use JSON-backed repositories
      - call the existing service layer
      - print the created or updated object id plus the key status/result fields
  - Keep inputs explicit and close to existing service signatures rather than inventing a higher-level DSL.
  - Include order_intent_id as an optional input to record-fill.
  - Keep demo-planned-trade intact for now as a smoke/demo path.
  - Do not add edit/update/delete commands in this issue.
  - Do not add interactive prompts; use explicit flags/arguments only.

  ### Public Interface Changes

  New CLI commands:

  - uv run trading-system create-trade-idea ...
  - uv run trading-system create-trade-thesis ...
  - uv run trading-system create-trade-plan ...
  - uv run trading-system approve-trade-plan <id>
  - uv run trading-system evaluate-trade-plan-rules <id>
  - uv run trading-system create-order-intent ...
  - uv run trading-system open-position <trade-plan-id>
  - uv run trading-system record-fill ...
  - uv run trading-system create-trade-review ...

  ### Test Plan

  - each command succeeds with valid inputs and persists data
  - commands reject invalid UUIDs with clear Typer errors
  - commands reject missing linked records with service-layer messages surfaced clearly
  - record-fill works both with and without order_intent_id
  - create-order-intent rejects plans without passing persisted rule evaluations
  - end-to-end CLI flow can create a position and review without using demo-planned-trade

  ## Issue 14: CLI Usability and Read-Side Polish

  ### Summary

  After the core write commands exist, improve CLI usability so the system is practical for repeated manual use.

  ### Key Changes

  - Improve read commands to surface the most useful fields now that users can create data incrementally:
      - include realized P&L in show-position
      - optionally include realized P&L column in list-positions for closed positions
      - keep timeline command unchanged in scope, but align output formatting with the rest of the CLI
  - Add lightweight list/read commands for upstream objects needed for command chaining:
      - list-trade-ideas
      - list-trade-plans
      - show-trade-plan
  - Recommended default: do not add thesis-specific list/show commands yet unless implementation reveals they are necessary for daily use.
  - Make CLI output consistent:
      - stable field naming
      - clear printing of ids needed for subsequent commands
  ### Public Interface Changes

  New CLI read commands:

  - uv run trading-system list-trade-ideas
  - uv run trading-system list-trade-plans
  - uv run trading-system show-trade-plan <id>

  Output refinements:

  - list-positions may add realized P&L for closed positions
  - show-position includes realized P&L
  - command output format becomes more consistent across write and read commands

  ### Test Plan

  - list/show commands return persisted ideas and plans in stable order
  - show-trade-plan includes approval state and key planning fields
  - list-positions formatting remains readable with and without closed positions
  - README examples match actual command names and argument shapes
  - existing retrieval commands still pass unchanged behavior where intended

  ## Assumptions and Defaults

  - show-position-timeline is considered complete from Issue 10 and should not be re-opened unless a later issue explicitly expands it.
  - Realized P&L should remain read-side only for now; do not persist computed P&L fields.
  - CLI usability should be split into two issues:
      - first for core write capability
      - then for chaining, discoverability, and read-side polish
  - OrderIntent should be included in the new write CLI because it is now part of the implemented workflow.
  - No interactive CLI mode, no bulk import, no edit/delete commands, and no advanced analytics are included in these three issues.
