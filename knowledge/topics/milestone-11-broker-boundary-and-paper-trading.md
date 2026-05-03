---
title: Milestone 11 Broker Boundary And Paper Trading
type: topic
status: complete
tags: [trading-system, milestone-11, broker-boundary, paper-trading, alpaca]
created: 2026-05-03
updated: 2026-05-03
---

# Milestone 11 Broker Boundary And Paper Trading

Milestone 11 is complete in the linked application repo.

It added the first broker execution boundary while staying paper-only, provider-agnostic, and CLI/core-services-only. The milestone proved simulated paper execution without live Alpaca calls, FastAPI broker controls, React broker controls, real-money execution, autonomous trading, recommendations, or full OMS behavior.

## Implemented Shape

Milestone 11 implemented:

- ADR-011 Broker Execution Boundary
- provider-agnostic `BrokerClient` port
- simulated paper broker adapter with no external network calls
- local `BrokerOrder` domain record
- `BrokerOrderRepository` protocol
- JSON and in-memory broker-order repositories
- `broker_orders` JSON collection
- optional `Fill.broker_order_id`
- `BrokerExecutionService.submit_paper_order`
- `BrokerExecutionService.sync_paper_order`
- idempotent broker-fill import
- lifecycle events: `BROKER_ORDER_SUBMITTED`, `BROKER_ORDER_FILLED`
- CLI commands: `submit-paper-order`, `sync-paper-order`, `show-broker-order`

Recorded application repo validation on 2026-05-03:

```text
Focused broker/order-intent/fill/persistence/CLI tests: 71 passed
Full suite: 257 passed
```

Live Alpaca paper submission remains deferred. Milestone 11 reserved `ALPACA_API_KEY` and `ALPACA_SECRET_KEY` for future vault-first, environment-fallback resolution.

## Key Decisions

Surface scope:

- core services and CLI only
- no API or web controls in Milestone 11

Position handling:

- require an existing local position before broker fills can be imported
- do not auto-open positions from broker events in this milestone
- do not open a position at order-submit time

Adapter depth:

- simulated paper broker first
- Alpaca-ready port first
- live Alpaca paper adapter later

Source of truth:

- broker data is external execution fact
- local JSON remains the source for internal trade records, lifecycle, and audit meaning

## Broker Order Boundary

Milestone 11 added a local `BrokerOrder` record for external paper order facts.

Expected fields:

- local id
- `order_intent_id`
- `position_id`
- provider
- provider order id
- symbol
- side
- order type
- quantity
- optional limit and stop prices
- status
- submitted timestamp
- updated timestamp

Milestone 11 status values should stay limited to:

- `submitted`
- `filled`
- `canceled`
- `rejected`

One `OrderIntent` has at most one broker order in this milestone.

`Fill` now has optional `broker_order_id`. Existing manual fills continue to load with `broker_order_id = None`; broker-imported simulated fills use `source = "broker:simulated"`.

## Expected Workflow

The Milestone 11 workflow is:

```text
approve plan -> evaluate rules -> create OrderIntent -> open local position -> submit paper order -> sync/import fill into that position
```

This preserves the current rule that local positions originate from the local trade lifecycle, not from broker events. A later milestone may reconsider auto-opening a position on first broker fill after broker-order/fill mapping has been proven.

## Service Behavior

`submit_paper_order(order_intent_id, position_id, provider="simulated")`:

- reject missing or canceled order intents
- reject missing or closed positions
- reject position/order-intent trade-plan mismatches
- reject duplicate broker orders for the same order intent
- store a `BrokerOrder`
- emit `BROKER_ORDER_SUBMITTED`

`sync_paper_order(broker_order_id, simulated_fill_price)`:

- update broker order status to filled
- record one local fill linked to `order_intent_id` and `broker_order_id`
- update the existing position through current fill logic
- emit `BROKER_ORDER_FILLED` and existing fill/position lifecycle events
- be idempotent so repeated sync does not create duplicate fills

Requiring an explicit simulated fill price keeps Milestone 11 tests and demos deterministic.

## Guardrails

Milestone 11 did not add:

- real-money execution
- autonomous or scheduled trading
- strategy signals
- generated recommendations
- broker-driven trade meaning
- full order management system behavior
- live Alpaca paper API calls in the first slice
- browser execution controls
- FastAPI broker endpoints
- portfolio-level reconciliation
- margin, buying-power, or risk engine expansion beyond narrow paper-trading checks
- cloud deployment or production auth

Real-money usage remains a later readiness gate, not an implementation milestone.

## Source-Of-Truth Boundary

Broker data can answer:

- what paper order request was sent from an approved `OrderIntent`
- what paper order was submitted
- what the broker accepted, rejected, filled, or canceled
- what the broker reports as account, order, fill, or raw position state

The trading system still answers:

- why the trade exists
- which thesis and playbook justify it
- whether rules were evaluated
- how the trade is reviewed
- what the internal lifecycle means
- which local records represent the internal audit trail

Any mismatch between broker facts and local state should be surfaced explicitly rather than silently corrected.

## Deferred Questions

- Whether a later live Alpaca paper adapter should support only market orders first or include limit/stop prices from `OrderIntent`.
- Whether future broker synchronization should stay explicit or later add polling.
- Whether browser execution controls belong in Milestone 12 or a later product workbench milestone.
- Whether auto-opening a local position on first broker fill is worth adding after the broker-order/fill mapping is proven.
- How much canceled/rejected paper order state should map back into `OrderIntent` status versus remaining on `BrokerOrder`.

## Processed Source

- [[proposed-milestone-11-broker-boundary-paper-execution-20260503]]
- [[post-milestone-11-higher-level-roadmap-20260503]]

## Verification Notes

Checked on 2026-05-03:

- `C:\Users\bosto\dockerstuff\trading-system\DOCS\ADR\011-broker-execution-boundary.md` exists and is accepted.
- `C:\Users\bosto\dockerstuff\trading-system\DOCS\milestone-11-issue-map.md` marks 11A through 11E complete.
- Application repo `STATUS.md` marks Milestones 1 through 11 complete.

## Related Pages

- [[milestone-10-secure-credentials]]
- [[post-milestone-11-roadmap]]
- [[canonical-domain-model]]
- [[trade-lifecycle-and-objects]]
- [[data-and-platform-strategy]]
- [[deterministic-rules-vs-contextual-intelligence]]
- [[application-implementation-status]]
- [[product-roadmap-and-learning-boundaries]]
