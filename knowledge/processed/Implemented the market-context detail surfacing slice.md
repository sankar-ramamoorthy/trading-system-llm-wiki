• Implemented the market-context detail surfacing slice.

  Changed:

  - Added market_context_snapshots to TradePlanDetail, PositionDetail, and TradeReviewDetail.
  - Wired MarketContextSnapshotRepository into the three query services and CLI service factories.
  - Added metadata-only Market context sections to:
      - show-trade-plan
      - show-position
      - show-trade-review
  - Kept full payload output limited to show-context.
  - Updated README.md, STATUS.md, and PROJECT.md.
  - Added service and CLI regression tests for linked-only snapshots and empty-state rendering.

  Verification:

  - Focused suite: 55 passed
  - Full suite: 123 passed via uv run pytest