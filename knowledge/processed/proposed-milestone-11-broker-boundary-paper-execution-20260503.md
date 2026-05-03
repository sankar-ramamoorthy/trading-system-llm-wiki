---
title: Proposed Milestone 11 Broker Boundary Paper Execution
type: processed-note
status: superseded
tags: [trading-system, milestone-11, broker-boundary, paper-trading, order-intent, positions]
created: 2026-05-03
---

# Proposed Milestone 11 Broker Boundary Paper Execution

This note processes the raw note `Proposed Milestone 11 plan.md`.

## Status

Milestone 11 was the next planned slice after completed Milestone 10 Secure Credentials when this note was first processed. It has since been implemented and closed in the linked application repo.

The raw note narrows the application repo roadmap's candidate broker-boundary direction into a core-and-CLI implementation plan. It is planning material, not an implementation closeout.

## Superseding Update

This processed plan was superseded later on 2026-05-03 by the completed Milestone 11 implementation.

Current application repo state:

- `DOCS/ADR/011-broker-execution-boundary.md` exists and is accepted.
- `DOCS/milestone-11-issue-map.md` marks 11A through 11E complete.
- `STATUS.md` marks Milestone 11 complete.
- Recorded validation: 71 focused broker/order-intent/fill/persistence/CLI tests passed and 257 full-suite tests passed.

The planning guidance below remains useful for rationale, but [[milestone-11-broker-boundary-and-paper-trading]] and [[post-milestone-11-roadmap]] are the current durable pages.

## Decisions Captured

Milestone 11 should use these resolved planning choices:

- Surface: core services and CLI only.
- Position handling: require an existing local position before broker fills can be imported.
- Adapter depth: implement a simulated paper broker and an Alpaca-ready provider port, but defer live Alpaca paper calls.
- Source of truth: broker data is external execution fact; local JSON remains the source for internal trade records and lifecycle audit.

This changes the earlier broad possibility of "CLI plus API plus web" into a narrower execution-boundary milestone. Browser execution controls belong in a later milestone after the broker boundary is proven.

## Recommended Issue Shape

Milestone 11 should likely be split as:

- 11A: ADR-011 Broker Execution Boundary.
- 11B: broker order domain and persistence boundary.
- 11C: simulated paper broker adapter and execution service.
- 11D: CLI commands for paper order submission, sync, and inspection.
- 11E: closeout docs, status, and validation.

The raw note explicitly rejects including FastAPI endpoints and React controls in this milestone.

## Domain Direction

Add a local `BrokerOrder` domain record as the internal representation of an external paper order fact.

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

Milestone 11 status values should stay narrow:

- `submitted`
- `filled`
- `canceled`
- `rejected`

One `OrderIntent` should map to at most one `BrokerOrder` in this milestone.

Extend `Fill` with optional `broker_order_id`. Existing manual fills continue to load with `broker_order_id = None`. Broker-imported simulated fills should use `source = "broker:simulated"` and remain linked to both the `OrderIntent` and `BrokerOrder`.

## Service Direction

Add a broker client port with submit and sync/status retrieval behavior. Implement a deterministic simulated paper broker adapter first.

`submit_paper_order(order_intent_id, position_id, provider="simulated")` should:

- reject missing or canceled order intents
- reject missing or closed positions
- reject position/order-intent trade-plan mismatches
- reject duplicate broker orders for the same order intent
- store a `BrokerOrder`
- emit `BROKER_ORDER_SUBMITTED`

`sync_paper_order(broker_order_id, simulated_fill_price)` should:

- update broker order status to filled
- record one local fill linked to `order_intent_id` and `broker_order_id`
- update the existing position through current fill logic
- emit `BROKER_ORDER_FILLED` plus the existing fill and position lifecycle events
- be idempotent so repeated sync does not create duplicate fills

Requiring `simulated_fill_price` keeps tests and demos explicit.

## Position Handling Decision

Milestone 11 should require a position before submit/sync.

Accepted workflow:

```text
approve plan -> evaluate rules -> create OrderIntent -> open local position -> submit paper order -> import fills into that position
```

This preserves the current model: a position exists because the local system opened it from an approved plan, not because a broker event created trade meaning.

Deferred improvement:

- Auto-open on first broker fill may be reconsidered later once broker-order/fill mapping is proven.

Rejected for Milestone 11:

- Open position on submit. Orders can be rejected, canceled, or remain unfilled, so submit-time position creation would create internal position state before execution fact exists.

## CLI Direction

The processed plan expects CLI commands for:

- submitting a paper order from an existing order intent and position
- syncing a paper broker order with an explicit simulated fill price
- showing broker order metadata

CLI output should include useful ids and status without exposing credentials or secrets.

## Test Direction

Unit tests should cover:

- cannot submit missing or canceled order intent
- cannot submit against missing, closed, or mismatched position
- cannot submit duplicate broker order for one order intent
- successful submit stores `BrokerOrder` and lifecycle event
- sync records one broker-sourced fill and updates position quantity
- repeated sync is idempotent
- broker fill can close a position through existing position logic
- JSON persistence round-trips `BrokerOrder`
- older fills without `broker_order_id` still load

CLI tests should cover:

- submit prints broker order id and status
- sync prints fill id, broker order id, position state, and open quantity
- show-broker-order displays metadata without secrets

Regression should include focused broker/order-intent/fill/persistence/CLI tests plus the full application test suite.

## Relationship To App Roadmap

The application repo `DOCS/product-roadmap.md` defined Milestone 11 as the next planned broker-boundary and paper-trading slice at the time of initial planning. Its candidate direction mentioned an execution-boundary ADR, Alpaca as a first implementation candidate, paper order intent to fill recording, and local JSON position/fill records.

This processed note makes that candidate direction more concrete while narrowing the first implementation:

- simulated paper broker first
- Alpaca-ready port first
- live Alpaca paper submission later
- API/web controls later
- existing local position required before imported broker fills can affect position state

## Related Pages

- [[milestone-11-broker-boundary-and-paper-trading]]
- [[milestone-10-secure-credentials]]
- [[canonical-domain-model]]
- [[trade-lifecycle-and-objects]]
- [[data-and-platform-strategy]]
