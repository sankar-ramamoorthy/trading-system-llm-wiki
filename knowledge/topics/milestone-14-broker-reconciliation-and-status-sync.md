---
title: Milestone 14 Broker Reconciliation And Status Sync
type: topic
status: complete
tags: [trading-system, milestone-14, broker-boundary, reconciliation, alpaca, paper-trading]
created: 2026-05-03
updated: 2026-05-03
---

# Milestone 14 Broker Reconciliation And Status Sync

Milestone 14 is complete in the linked application repo.

The goal is explicit local-vs-broker reconciliation for paper orders. The system surfaces differences between local `BrokerOrder` records and remote Alpaca order facts without allowing broker data to redefine local trade meaning.

## Current Status

Application repo status on 2026-05-03 marks Milestones 1 through 14 complete. Milestone 14 implemented CLI-only broker reconciliation and status sync after the Alpaca paper adapter.

## Design Intent

Milestone 14 added a controlled reconciliation layer:

- provider order snapshots behind the broker port
- Alpaca order listing inside the infrastructure adapter
- batch sync of local submitted Alpaca broker orders
- idempotent fill import when the remote order is filled
- local broker-order status updates for remote submitted, canceled, rejected, and filled states
- explicit mismatch reporting when local and remote facts disagree
- lifecycle audit events for sync and reconciliation outcomes

The important boundary is that broker facts remain external execution facts. Local JSON records remain the source of truth for internal audit, trade lifecycle, and trade meaning.

## Broker Snapshot

The broker port exposes a provider-neutral `BrokerOrderSnapshot` with:

- provider order id
- mapped local broker status
- updated timestamp
- symbol
- side
- quantity
- optional average fill price

Provider SDK response objects stay inside infrastructure. Alpaca-specific objects do not leak into domain services, repositories, CLI output contracts, or durable local records.

## Service Behavior

`BrokerReconciliationService` supports:

- syncing all local submitted broker orders for a provider
- importing exactly one local fill when a remote order is filled
- updating local `BrokerOrder` status from remote broker status
- reporting local orders missing from the broker response
- reporting broker-only remote orders without persisting them as local orders
- reporting status and fill mismatches without deleting local records or changing local trade meaning

Broker-only remote orders should remain report-only in Milestone 14. Local order creation must continue to originate from local `OrderIntent` and `Position` records.

## CLI Surface

```text
sync-broker-orders --provider alpaca
reconcile-broker-orders --provider alpaca
```

`sync-broker-orders` explicitly syncs local submitted Alpaca broker orders and prints per-order results.

`reconcile-broker-orders` compares local broker orders to remote broker snapshots and reports counts for:

- matched
- updated
- missing remote
- broker-only
- status mismatch
- fill mismatch

## Non-Goals

Milestone 14 did not add:

- real-money execution
- FastAPI broker visibility or controls
- React broker visibility or controls
- browser submit, sync, cancel, or reconcile buttons
- broker positions as canonical local positions
- automatic scheduled sync, polling, streaming, webhooks, or background jobs
- automatic repair of local `Position`, `OrderIntent`, or `Fill` records beyond the explicit fill import path

## Validation

Validation recorded on 2026-05-03:

- `uv run pytest tests\test_alpaca_paper_broker.py tests\test_broker_reconciliation_service.py tests\test_broker_execution_service.py tests\test_cli_workflow_commands.py`: 51 passed
- `uv run pytest`: 287 passed

## Related Pages

- [[proposed-milestone-14-broker-reconciliation-status-sync-20260503]]
- [[implemented-milestone-14-broker-reconciliation-status-sync-20260503]]
- [[milestone-13-alpaca-paper-adapter]]
- [[milestone-11-broker-boundary-and-paper-trading]]
- [[post-milestone-11-roadmap]]
- [[application-implementation-status]]
