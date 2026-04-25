 Proposed Plan


  ## Add Order Intent Cancellation

  ### Summary

  Implement a narrow Milestone 3 follow-on that adds explicit OrderIntent cancellation without expanding the domain beyond the current manual workflow
  model.

  This issue should:

  - add a new OrderIntentStatus.CANCELED
  - add a cancel-order-intent <order-intent-id> CLI command
  - persist canceled status in memory and JSON repositories
  - emit an audit LifecycleEvent when cancellation happens
  - block downstream use of canceled order intents where they are currently consumed

  This issue should not add dedicated order-intent list/show commands. Existing read surfaces that already display order intents should simply show the
  updated status.

  ### Key Changes

  - Domain and persistence:
      - Extend OrderIntentStatus from created only to created|canceled.
      - Keep OrderIntent immutable/frozen; cancellation should be implemented by creating an updated OrderIntent instance with the same identity and
        timestamps but status=canceled.
      - Add update(order_intent: OrderIntent) to OrderIntentRepository.
      - Implement update() in in-memory and JSON repositories and ensure JSON round-trip supports the new enum value.
      - Do not add new timestamps or new fields for cancellation in this issue.
  - Cancellation service behavior:
      - Add a dedicated service, separate from creation, responsible for canceling an existing order intent.
      - Cancellation rules:
          - missing order intent: raise the existing style of ValueError
          - already canceled: reject with a clear idempotency-style error rather than silently succeeding
          - created -> canceled is the only new transition in this issue
      - On successful cancellation, persist the updated order intent and emit a lifecycle event on entity_type="OrderIntent" with
        event_type="ORDER_INTENT_CANCELED".
      - Event details should at minimum include order_intent_id, trade_plan_id, and resulting status.
  - Downstream enforcement:
      - Update FillService.record_manual_fill() so a provided order_intent_id must not be canceled.
      - If a canceled order intent is referenced from a fill, reject with a clear error before any fill is recorded.
      - Do not change PositionService.open_position_from_plan() because positions open from TradePlan, not from OrderIntent, and there is no current
        direct order-intent dependency there.
      - Do not invent broader execution-state semantics beyond this explicit canceled-state guard.
  - CLI and read-side behavior:
      - Add cancel-order-intent <order-intent-id> to the Typer CLI.
      - Output should follow the existing compact chaining style used by write commands and include at least:
          - order_intent_id
          - trade_plan_id
          - status
      - Preserve invalid UUID and missing-record CLI behavior patterns already used elsewhere.
      - README should be updated to include the new command and to remove the current “not in this issue” framing for cancellation.

  - Order-intent workflow tests:
      - can cancel an existing created order intent
      - cancel persists status change
      - cancel emits ORDER_INTENT_CANCELED
      - cannot cancel a missing order intent
      - cannot cancel an already canceled order intent
      - canceled order intent cannot be linked from manual fill
      - non-canceled order intent linkage still works unchanged
  - Persistence tests:
      - JSON repository round-trips OrderIntentStatus.CANCELED
      - OrderIntentRepository.update() survives reload in JSON
      - in-memory repository update() replaces the stored record correctly
  - CLI tests:
      - cancel-order-intent success output
      - invalid UUID handling
      - missing-record handling
      - read-side detail views show status: canceled where order intents already appear
  - Regression checks:
      - existing order-intent creation tests remain green
      - existing plan/position/review retrieval tests remain green
      - full uv run pytest

  ### Assumptions And Defaults

  - Use command name cancel-order-intent rather than the user’s typo concel-order-intent.
  - Cancellation is a narrow audit and guardrail feature, not a full execution-state machine.
  - The only new status in this issue is canceled.
  - “Block all downstream” is implemented against actual current downstream usage: fill linkage. No extra behavior should be invented where the current
    code has no order-intent dependency.
  - No dedicated order-intent retrieval commands are added in this issue; only existing read surfaces reflect the new status.