
• Implemented the Milestone 4 local context snapshot slice.

  What changed:

  - Added MarketContextSnapshot domain record and repository/source ports.
  - Added local JSON file import adapter.
  - Added JSON and in-memory snapshot repositories.
  - Added import/query services with target validation and instrument derivation.
  - Added CLI commands:
      - import-context
      - list-context
      - show-context
  - Updated docs/status in README.md, STATUS.md, PROJECT.md, and DOCS/milestone-4-market-context-design.md.
  - Added tests for service behavior, persistence/import validation, and CLI workflows.

  Validation:

  - Focused tests: 13 passed
  - Full suite: 117 passed

  Notably, yfinance is still not implemented. The code now has the source boundary needed for a future provider, and the
  docs state that adding yfinance or another external provider should get a small ADR first.
