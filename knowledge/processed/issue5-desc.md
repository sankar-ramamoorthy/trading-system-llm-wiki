---
title: Issue 5 Close Position From Fills Description
type: status
status: processed
created: 2026-04-19 21:51:54 -04:00
tags: [trading-system, milestone-1, issue-5, positions, fills]
---

# Issue 5 - Close Position From Fills

Implemented automatic position closing as a consequence of recorded fills in the application repo:

C:\Users\bosto\dockerstuff\trading-system

## Summary

Issue 5 extends manual fill recording so a position closes naturally when reducing fills bring current_quantity to exactly zero. No separate close_position() workflow was added. Closing remains a domain state transition caused by execution data.

## Main Changes

- Extended Position with close metadata:
  - closing_fill_id
  - close_reason
- Updated Position.record_fill(fill) so a reducing sell fill that brings open quantity to zero:
  - sets lifecycle_state = "closed"
  - sets closed_at = fill.filled_at
  - records the closing fill id
  - sets close_reason = "fills_completed"
- Preserved oversell/reversal rejection.
- Preserved rejection of any further fills after the position is closed.
- Updated FillService.record_manual_fill(...) to detect an open-to-closed transition and emit a distinct POSITION_CLOSED lifecycle event.
- Extended SQLAlchemy model scaffold with close metadata fields.
- Extended CLI demo so uv run trading-system demo-planned-trade records an entry fill and an exit fill, leaving the position closed.
- Updated README to describe the close-from-fills workflow.

## Audit Behavior

A closing fill records both:

- FILL_RECORDED
- POSITION_CLOSED

The POSITION_CLOSED event includes structured details such as:

- position id
- closed timestamp
- closing fill id
- resulting current quantity
- close reason

## Validation

Ran successfully:

`	ext
python -m compileall src tests scripts
uv run pytest
uv run trading-system demo-planned-trade
`

Test result:

`	ext
23 passed
`

CLI demo now reports a closed position:

`	ext
fills=2 open_quantity=0 state=closed lifecycle_events=4
`

## Scope Preserved

No broker integration, market data, P&L engine, trade review, force-close workflow, reopen workflow, reversal logic, or analytics were added.
