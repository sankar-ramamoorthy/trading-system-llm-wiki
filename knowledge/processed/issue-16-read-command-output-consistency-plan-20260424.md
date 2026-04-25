---
title: Issue 16 Read Command Output Consistency Plan
type: note
status: active
tags: [trading-system, cli, milestone-3, issue-16]
created: 2026-04-24
updated: 2026-04-24
---

# Issue 16 Read Command Output Consistency Plan

Issue 16 is the next immediate Milestone 3 usability task for the application repo at `C:\Users\bosto\dockerstuff\trading-system`.

## Summary

Implement a narrow read-command presentation cleanup so the existing CLI output is consistent, easier to scan, and more repeatable for manual use.

This is a presentation-only slice:

- no new commands
- no new flags
- no new filters
- no domain-model changes
- no market/context work
- no write-command cleanup in this issue

Target commands:

- `list-trade-ideas`
- `list-trade-plans`
- `show-trade-plan`
- `list-trade-reviews`
- `show-trade-review`
- `list-positions`
- `show-position`
- `show-position-timeline`

## Output Standardization Goals

### List commands

- Keep exactly one header row per list command.
- Put the primary entity id first in every row.
- Put timestamps last in every row.
- Keep column order fixed and deterministic.
- Use one empty-state pattern per entity type:
  - `No trade ideas found.`
  - `No trade plans found.`
  - `No trade reviews found.`
  - `No positions found.`

### Show commands

- First line is always `<Entity label> <id>`.
- Top-level identity and status fields come first.
- Linked sections follow in a fixed order.
- Use one blank line between sections.
- Inside show sections, use `field_name: value` lines only.

### Section order

`show-trade-plan`

1. top-level plan fields
2. `Trade idea`
3. `Trade thesis`
4. `Rule evaluations`
5. `Order intents`
6. `Positions`

`show-trade-review`

1. top-level review fields
2. `Position`
3. `Trade plan`
4. `Trade idea`

`show-position`

1. top-level position fields
2. `Trade plan`
3. `Trade idea`
4. `Order intents`
5. `Fills`
6. `Review`

## Formatting Rules

- Optional decimal values use `N/A`.
- Optional datetime values use empty string in list output.
- Show output should stay explicit and use `N/A` only where needed for clarity.
- Optional text values use empty string, not `None`.
- Repeated string fields use `; ` consistently.
- Timestamps remain ISO formatted.
- Ratings in list output remain blank when missing.

## Timeline Constraint

`show-position-timeline` should keep its current tabular layout and chronological ordering, with a single header row and output wording aligned with the rest of the read CLI.

## Test Expectations

Update or tighten CLI retrieval tests to assert:

- exact header text for list commands
- section order for show commands
- consistent empty-state messages
- ISO timestamp rendering
- normalized optional-value formatting
- unchanged invalid UUID and missing-record error behavior

Minimum validation target after implementation:

- `tests/test_cli_retrieval.py`
- `tests/test_cli_workflow_commands.py`
- full `uv run pytest`
