• Proposed Plan


  # Surface Market Context In Detail Views

  ## Summary

  Extend the current Milestone 4 local context snapshot slice so stored snapshots appear directly inside the existing
  planning and review inspection workflows. This completes the practical “context alongside the trade” behavior
  without adding external providers or changing canonical trade meaning.

  ## Implementation Changes

  - Extend read models:
      - Add market_context_snapshots: list[MarketContextSnapshot] to TradePlanDetail, PositionDetail, and
        TradeReviewDetail.
      - Inject MarketContextSnapshotRepository into TradeQueryService, PositionQueryService, and ReviewQueryService.
  - Retrieval behavior:
      - show-trade-plan loads snapshots linked to target_type="TradePlan" and that plan ID.
      - show-position loads snapshots linked to target_type="Position" and that position ID.
      - show-trade-review loads snapshots linked to target_type="TradeReview" and that review ID.
      - Sort snapshots by captured_at oldest first, matching existing chronological read output.
  - CLI rendering:
      - Add a Market context section to each detail command.
      - For each snapshot, show market_context_snapshot_id, context_type, source, source_ref, observed_at, and
        captured_at.
      - Do not print full payload in embedded detail views; keep payload inspection in show-context to avoid noisy
        trade views.
      - Empty section text: No market context snapshots found.
  - Documentation:
      - Update README.md examples to show importing context and then viewing it through show-trade-plan, show-
        position, or show-trade-review.

  - Service tests:
      - TradeQueryService.get_trade_plan_detail includes only snapshots linked to that trade plan.
      - PositionQueryService.get_position_detail includes only snapshots linked to that position.
      - ReviewQueryService.get_trade_review_detail includes only snapshots linked to that review.
      - Existing detail behavior remains unchanged when no snapshots exist.
  - CLI tests:
      - show-trade-plan renders a Market context section with linked snapshot metadata.
      - show-position renders linked position context.
      - show-trade-review renders linked review context.
      - Empty detail views show No market context snapshots found.
  - Regression:
      - Run focused retrieval/context tests.
      - Run full test suite.

  ## Assumptions

  - Build on the current uncommitted local context import slice.
  - Do not add yfinance, external providers, new dependencies, or an ADR in this slice.
  - Context stays advisory and read-only; no domain entity, lifecycle state, rule evaluation, fill, or review content
    is mutated by surfacing snapshots.