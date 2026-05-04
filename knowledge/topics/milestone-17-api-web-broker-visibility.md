---
title: Milestone 17 API/Web Broker Visibility
type: topic
status: proposed
tags: [trading-system, milestone-17, broker-boundary, paper-trading, web, api]
created: 2026-05-04
updated: 2026-05-04
---

# Milestone 17 API/Web Broker Visibility

Milestone 17 is proposed as read-only browser and API visibility for broker execution facts.

It should make broker state visible in the web app without allowing the browser to submit, sync, cancel, reconcile, or otherwise mutate broker state.

## Intent

Milestone 17 is about operational awareness.

The user should be able to inspect broker order state from the browser instead of relying only on CLI commands such as:

```text
list-broker-orders
show-broker-order
sync-broker-orders
reconcile-broker-orders
```

The milestone should prove that broker state can be displayed clearly before the system adds browser execution controls.

## Proposed Surface

FastAPI and React should expose read-only views of:

- broker order status: submitted, filled, canceled, rejected
- provider: simulated or Alpaca
- provider order id
- linked `OrderIntent`
- linked `Position`
- linked fills
- fill price and quantity
- submitted, updated, and synced timestamps
- reconciliation and mismatch information from local vs broker state

The browser may show broker-order detail in plan and position views, or through a dedicated broker-orders view, but all behavior should remain read-only.

## Boundary

Milestone 17 should not add:

- browser paper submission
- browser sync
- browser cancellation
- browser reconciliation
- real-money execution
- generated recommendations
- autonomous or scheduled broker actions
- trade creation from broker state

`BrokerOrder` and `Fill` remain local audit records. Broker data remains external execution fact. Browser visibility must not redefine local trade meaning.

## Related Pages

- [[high-level-milestone-17-and-milestone-18-20260504]]
- [[milestone-18-browser-paper-execution-controls]]
- [[milestone-14-broker-reconciliation-and-status-sync]]
- [[milestone-11-broker-boundary-and-paper-trading]]
- [[post-milestone-11-roadmap]]
