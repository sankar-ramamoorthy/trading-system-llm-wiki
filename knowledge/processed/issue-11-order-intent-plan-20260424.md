• One implementation choice materially changes the plan: whether Issue 11 should only insert OrderIntent into the existing workflow, or also move Position
  creation later to line up with the conceptual lifecycle you wrote. I can recommend the narrower path, but I want to lock that explicitly before finalizing
  the spec.

• Questions 1/1 answered
  • For Issue 11, should `Position` creation stay where it is today, with `OrderIntent` inserted before fills, or should this issue also move `Position`
    creation to happen from the first fill?
    answer: Keep current timing (Recommended)

• Proposed Plan


  # Issue 11 Plan: Introduce OrderIntent Narrowly

  ## Summary

  Implement OrderIntent as a first-class domain object between approved TradePlan and manual Fill, while keeping current Position creation timing
  unchanged for this issue.

  This means Issue 11 will:

  - add a persisted OrderIntent entity plus enums
  - require approved plan + successful rule evaluation before creating an order intent
  - allow Fill to optionally reference an OrderIntent
  - surface linked order intents in retrieval output
  - update the demo to create an order intent before recording fills

  It will not turn OrderIntent into a broker order, and it will not refactor Position to be created from first fill in this issue.

  ## Key Changes

  ### Domain and repository contracts

  - Add domain/trading/order_intent.py with:
      - OrderIntent
      - OrderSide
      - OrderType
      - OrderIntentStatus
  - Keep the entity simple and local to system intent only:
      - id, trade_plan_id, symbol, side, order_type, quantity
      - limit_price, stop_price
      - status, created_at, notes
  - Extend Fill with order_intent_id: UUID | None = None.
  - Export OrderIntent from domain/trading/__init__.py.
  - Add OrderIntentRepository to ports/repositories.py with:
      - add(order_intent)
      - get(order_intent_id)
      - list_by_trade_plan_id(trade_plan_id)

  ### Rule-evaluation gating

  - Extend RuleEvaluationRepository with a read method:
      - list_by_entity(entity_type, entity_id)
  - Use this in the new order-intent service to enforce:
      - plan exists
      - plan is approved
      - at least one RuleEvaluation exists for that plan
      - all persisted evaluations for that plan passed
  - Reject order-intent creation if no evaluations exist or if any evaluation failed.
  - Do not change PositionService.open_position_from_plan(...) in Issue 11.

  ### Services and workflow wiring

  - Add CreateOrderIntentService in services/.
  - Service input should be explicit and narrow:
      - trade_plan_id
      - symbol
      - side
      - order_type
      - quantity
      - optional limit_price, stop_price, notes
  - Service behavior:
      - load the TradePlan
      - validate approval and rule evaluations
      - create OrderIntent with status=CREATED
      - persist it
      - record a lifecycle event
  - Lifecycle event shape:
      - entity_type="OrderIntent"
      - event_type="ORDER_INTENT_CREATED"
      - note references the plan
      - details include trade_plan_id, symbol, side, order_type, quantity, prices, and status
  - Update FillService.record_manual_fill(...) to accept optional order_intent_id.
  - When order_intent_id is provided:
      - load the order intent
      - ensure it exists
      - ensure its trade_plan_id matches the position’s trade_plan_id
      - persist the fill with that link
  - Do not auto-update order intent status beyond storing the fill link in this issue; status transitions beyond creation remain out of scope unless
    needed for a minimal fill-linked update.
  - Keep order_intent_id=None valid for legacy/manual cases and older persisted data.

  ### Retrieval, JSON, and demo

  - Add in-memory and JSON repository support for OrderIntent.
  - Extend JSON store collections with order_intents.
  - Add JSON serialization/deserialization for:
      - OrderIntent
      - Fill.order_intent_id
      - RuleEvaluationRepository.list_by_entity(...)
  - Extend JsonRepositorySet and build_json_repositories(...) to expose order_intents.
  - Extend PositionQueryService.PositionDetail with order_intents: list[OrderIntent].
  - Populate order_intents by position.trade_plan_id, sorted by created_at.
  - Update show-position to display linked order intents before fills.
      - Show at least id, status, side, order_type, quantity, symbol, and any non-null prices.
  - Update demo-planned-trade so the sequence becomes:
      - create/approve plan
      - evaluate rules
      - create order intent
      - open position
      - record fills linked to that order intent
      - review
  - Keep show-position-timeline unchanged; order-intent lifecycle events remain queryable separately if needed later.

  ## Test Plan

  - cannot create OrderIntent when any persisted rule evaluation for the plan failed
  - can create OrderIntent from an approved plan with passing evaluations
  - OrderIntent persists and reloads through JSON unchanged
  - creating an order intent records ORDER_INTENT_CREATED
  - Fill can round-trip with order_intent_id
  - FillService accepts a valid linked order_intent_id
  - FillService rejects an order_intent_id whose plan does not match the position’s plan
  - PositionQueryService.get_position_detail(...) returns linked order intents
  - show-position output includes order-intent information
  - existing position state derivation from fills still works unchanged
  - existing fill persistence/retrieval tests still pass for order_intent_id=None

  ## Public Interfaces / Behavior Changes

  - New entity: OrderIntent
  - New service: CreateOrderIntentService
  - New repository port: OrderIntentRepository
  - Extended repository port: RuleEvaluationRepository.list_by_entity(...)
  - Extended fill shape: Fill.order_intent_id
  - Extended read model: PositionDetail.order_intents
  - Extended fill workflow API:
      - FillService.record_manual_fill(..., order_intent_id: UUID | None = None)

  ## Assumptions

  - Chosen default: keep current Position timing for Issue 11; do not move position creation to first fill yet.
  - Retrieval support will be added to show-position, not a new CLI command.
  - Rule gating means persisted rule-evaluation artifacts must already exist and all must have passed before an order intent can be created.
  - symbol on OrderIntent will be stored as a simple string supplied by the caller; this issue does not derive or normalize it through a richer instrument
    model.
  - Status lifecycle stays minimal in Issue 11; the critical requirement is creation, persistence, linkage to fills, and auditability.