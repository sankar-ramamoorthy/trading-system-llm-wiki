---
title: Proposed Milestone 14 Broker Reconciliation And Status Sync
type: processed-note
status: processed
tags: [trading-system, milestone-14, broker-boundary, reconciliation, alpaca, paper-trading]
created: 2026-05-03
processed: 2026-05-03
source: knowledge/raw/Proposed plan Milestone 14 Broker Reconciliation And Status Sync.md
---

# Proposed Milestone 14 Broker Reconciliation And Status Sync

## Summary

Milestone 14 is proposed as the explicit broker reconciliation slice after the Alpaca paper adapter.

The purpose is to make local-vs-broker differences visible without allowing broker data to redefine local trade meaning. Local `BrokerOrder`, `Fill`, and `Position` records remain canonical for internal audit. Alpaca order facts remain external execution facts used for explicit sync and mismatch reporting.

## Proposed Shape

- Add a broker snapshot shape to the broker port, such as `BrokerOrderSnapshot`.
- Include provider order id, mapped local status, updated time, symbol, side, quantity, and optional fill price in the snapshot shape.
- Extend the Alpaca adapter with provider order listing through installed `alpaca-py`.
- Keep provider response objects inside infrastructure.
- Add a `BrokerReconciliationService` for batch sync, idempotent fill import, status updates, and mismatch reporting.
- Add CLI commands for explicit broker sync and reconciliation.
- Record lifecycle audit events on local broker orders.
- Keep broker-only remote orders report-only in this milestone unless a local `BrokerOrder` already exists.

## Proposed CLI Surface

```text
sync-broker-orders --provider alpaca
reconcile-broker-orders --provider alpaca
```

`sync-broker-orders` should sync local submitted Alpaca broker orders and print per-order results.

`reconcile-broker-orders` should compare local broker orders to remote broker order snapshots and print counts for matched, updated, missing remote, broker-only, status mismatch, and fill mismatch cases.

## Boundary

Milestone 14 should not add:

- real-money execution
- FastAPI broker visibility or controls
- React broker visibility or controls
- browser submission, sync, cancel, or reconcile buttons
- broker positions as canonical local positions
- automatic scheduled sync, polling, streaming, webhooks, or background jobs
- automatic repair of local `Position`, `OrderIntent`, or `Fill` records beyond the existing explicit fill import path

## Test Direction

Expected tests:

- Alpaca remote order listing and snapshot mapping using fake Alpaca clients.
- Batch sync imports exactly one fill for filled remote orders.
- Repeated batch sync remains idempotent.
- Remote submitted, canceled, and rejected statuses update local `BrokerOrder` status.
- Local filled order with remote non-filled status reports a mismatch without deleting local fills.
- Local order missing from the remote broker report is surfaced.
- Broker-only remote order is surfaced but not persisted locally.
- Lifecycle audit events are emitted for syncs and local mismatches.
- CLI coverage for both proposed commands using fake broker clients.

## Assumptions

- Implementation belongs in `C:\Users\bosto\dockerstuff\trading-system`.
- Knowledge-base status should be updated after validation.
- Reconciliation is Alpaca-first.
- Simulated broker behavior remains available for existing tests and local demos, but does not need remote listing.
- Broker-only remote records should not create local orders, because local order creation must originate from local `OrderIntent` and `Position` records.

## Promoted Topic

- [[milestone-14-broker-reconciliation-and-status-sync]]
