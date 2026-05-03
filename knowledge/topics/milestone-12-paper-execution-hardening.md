---
title: Milestone 12 Paper Execution Hardening
type: topic
status: complete
tags: [trading-system, milestone-12, broker-boundary, paper-trading, cli]
created: 2026-05-03
updated: 2026-05-03
---

# Milestone 12 Paper Execution Hardening

Milestone 12 is complete in the linked application repo.

It hardened the simulated paper broker workflow before adding Alpaca, API broker endpoints, or browser broker controls. The implementation remained core services plus CLI only.

## Implemented Shape

Milestone 12 added:

- `BrokerQueryService`
- `list-broker-orders`
- broker-order filters by provider, status, position id, and order intent id
- oldest/newest broker-order sorting
- richer `show-broker-order` output with linked fill count and fill ids
- order-intent status, position state, and open quantity in broker-order detail
- broker-order linkage in `show-position` fill output
- broker-order ids in `show-position-timeline` lifecycle event details
- `cancel-paper-order`
- `reject-paper-order --reason`
- terminal `canceled` and `rejected` broker-order states
- lifecycle events: `BROKER_ORDER_CANCELED`, `BROKER_ORDER_REJECTED`

Canceled and rejected broker orders cannot be synced into fills.

## Boundary

Milestone 12 did not add:

- Alpaca
- FastAPI broker endpoints
- React broker controls
- real-money execution
- autonomous trading
- recommendations
- full OMS behavior

Broker data remains external execution fact. Local JSON remains the source of truth for internal trade records and audit history.

## Validation

Recorded on 2026-05-03:

```text
uv run pytest tests\test_broker_execution_service.py tests\test_json_persistence.py tests\test_cli_workflow_commands.py tests\test_cli_retrieval.py
85 passed

uv run pytest
264 passed
```

## Related Pages

- [[milestone-11-broker-boundary-and-paper-trading]]
- [[milestone-13-alpaca-paper-adapter]]
- [[post-milestone-11-roadmap]]
- [[application-implementation-status]]
