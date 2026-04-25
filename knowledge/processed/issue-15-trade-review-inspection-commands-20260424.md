
• Proposed Plan


  # Issue 15: Trade Review Inspection Commands

  ## Summary

  Plan the first Milestone 3 usability issue as a narrow review-inspection slice. OrderIntent, explicit write CLI commands, retrieval, and read-side
  realized P&L are already present in the current tree and the targeted tests pass, so the next issue should improve daily CLI usability rather than
  extend Milestone 2 again.

  This issue should add review-specific read workflows so a user can inspect completed reviews without opening the JSON store manually and without over-
  expanding into filtering, analytics, or edit/correction behavior.

  ## Key Changes

  - Add a dedicated read-side query service for reviews rather than overloading the write service or PositionQueryService.
      - Recommended name: ReviewQueryService
      - It should compose TradeReview, Position, TradePlan, and TradeIdea into review-oriented read models.
  - Extend the review persistence boundary to support review inspection by identity and listing.
      - TradeReviewRepository.get(review_id: UUID) -> TradeReview | None
      - TradeReviewRepository.list_all() -> list[TradeReview]
      - Implement both in memory and JSON repositories.
      - Keep get_by_position_id() unchanged because it is still needed by existing workflows.
  - Add two CLI commands:
      - list-trade-reviews
      - show-trade-review <trade-review-id>
  - Define the minimum review list/detail behavior:
      - list-trade-reviews returns reviews ordered by reviewed_at
      - each row should include at least:
          - trade_review_id
          - position_id
          - trade_plan_id
          - review rating
          - short summary
          - reviewed_at
      - include upstream planning context needed for manual inspection:
          - trade purpose
          - trade direction
      - if no reviews exist, print a clear empty-state message consistent with existing CLI commands
  - Define the review detail output:
      - print review identity and timestamps
      - print full structured review fields:
          - summary
          - what went well
          - what went poorly
          - lessons learned
          - follow-up actions
          - rating
      - print linked trade context:
          - position state
          - trade plan id
          - trade idea purpose/direction/horizon
          - realized P&L from the linked closed position, using the existing read-side calculation path
      - do not add timeline expansion, editing, tagging, export, or analytics in this issue
  - Keep output style aligned with the current CLI:
      - stable field names
      - ids printed explicitly for chaining
      - ISO timestamps
      - simple text output, no JSON mode and no interactive prompts

      - TradeReviewRepository.list_all() -> list[TradeReview]
  - New read service:
      - ReviewQueryService
      - read models for review list rows and review detail
  - New CLI commands:
      - uv run trading-system list-trade-reviews
      - uv run trading-system show-trade-review <trade-review-id>

  ## Test Plan

  - repository tests for JSON and in-memory review persistence support:
      - get(review_id) returns the persisted review
      - list_all() returns all reviews
  - query service tests:
      - review list is ordered by reviewed_at
      - review detail includes linked position, plan, idea, and realized P&L
      - missing review id raises a clear ValueError
  - CLI tests:
      - list-trade-reviews shows persisted reviews with ids and summary fields
      - show-trade-review <id> shows the full structured review and linked trade context
      - empty review store prints a consistent no-data message
      - invalid UUID input fails with the existing clear Typer UUID error
      - existing show-position and create-trade-review behavior remain unchanged

  ## Assumptions And Defaults

  - Treat the current OrderIntent and explicit CLI workflow slice as complete enough to move on.
  - This issue is inspection-only; no review editing, tagging, filtering, exports, or correction flows.
  - show-trade-review should key off trade_review_id, not position_id, because create-trade-review already emits trade_review_id for chaining.
  - Realized P&L remains read-side only and is reused in review detail rather than persisted onto the review.
  - Filters such as --rating, --has-review, or date ranges are deferred to a later Milestone 3 usability issue.
