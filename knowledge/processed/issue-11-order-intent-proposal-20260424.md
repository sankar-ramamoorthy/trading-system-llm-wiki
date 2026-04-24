## Issue 11 — Introduce `OrderIntent`

Yes. After persistence and retrieval, this is the right next domain issue.

## Purpose

Add the missing intent layer between an approved `TradePlan` and real-world `Fill`.

`Fill` should continue to mean **execution reality**.

`OrderIntent` should mean **what you intended to place or execute**.

## Core rule

The lifecycle should become:

```text
TradePlan → approval → RuleEvaluation → OrderIntent → Fill → Position → TradeReview
```

Not:

```text
TradePlan → Fill
```

## Scope

Add:

* `OrderIntent` domain entity
* repository interface + JSON repository support
* service to create an order intent from an approved trade plan
* lifecycle event when an order intent is created
* retrieval support in `show-position` or related output
* tests for approval gating and persistence

## `OrderIntent` fields

Keep it simple:

```python
@dataclass(frozen=True)
class OrderIntent:
    id: OrderIntentId
    trade_plan_id: TradePlanId
    symbol: str
    side: OrderSide
    order_type: OrderType
    quantity: Decimal
    limit_price: Decimal | None
    stop_price: Decimal | None
    status: OrderIntentStatus
    created_at: datetime
    notes: str | None = None
```

Enums:

```python
class OrderSide(Enum):
    BUY = "buy"
    SELL = "sell"

class OrderType(Enum):
    MARKET = "market"
    LIMIT = "limit"
    STOP = "stop"
    STOP_LIMIT = "stop_limit"

class OrderIntentStatus(Enum):
    CREATED = "created"
    CANCELLED = "cancelled"
    FILLED = "filled"
    PARTIALLY_FILLED = "partially_filled"
```

## Important boundary

Do **not** make this a broker order.

Avoid fields like:

* broker order id
* exchange route
* submitted timestamp
* order status from broker
* execution venue
* time-in-force complexity

Those belong later.

For now, this is your system’s intended action.

## Service

Add something like:

```python
CreateOrderIntentService
```

Responsibilities:

* load approved `TradePlan`
* ensure plan has passed rule evaluation
* create `OrderIntent`
* persist it
* record lifecycle event

It should **not** create a fill.

Manual fills remain separate.

## Fill relationship

Update `Fill` to optionally reference `OrderIntentId`.

```python
order_intent_id: OrderIntentId | None
```

Use `None` for legacy/manual fills if needed.

But going forward, new fills should ideally be recorded against an order intent.

## Tests

Minimum tests:

* cannot create `OrderIntent` from unapproved plan
* can create `OrderIntent` from approved plan
* created intent persists and reloads from JSON
* lifecycle event is recorded
* fill can reference order intent
* existing position derivation still works

## Non-goals

Do not add:

* broker integration
* order submission
* async order states
* cancellation workflows beyond simple status if needed
* bracket orders
* advanced routing
* position sizing engine
* realized P&L

## Acceptance criteria

Issue 11 is done when:

* `OrderIntent` exists as a first-class domain entity
* approved plan can produce one or more order intents
* fills can reference order intents
* existing lifecycle still works
* JSON persistence supports the new entity
* demo workflow uses `OrderIntent` before manual fill recording
* tests pass

## My recommendation

Keep Issue 11 narrow:

**Create `OrderIntent`, persist it, and route manual fills through it.**

Do not build a full order-management system yet.
