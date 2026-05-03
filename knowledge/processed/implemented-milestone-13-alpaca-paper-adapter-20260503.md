---
title: Implemented Milestone 13 Alpaca Paper Adapter
type: processed-note
status: processed
tags: [trading-system, milestone-13, alpaca, paper-trading, broker-boundary]
created: 2026-05-03
---

# Implemented Milestone 13 Alpaca Paper Adapter

This note processes the raw note `Implemented Milestone 13 in the app repo and updated the knowledge base.md`.

## Status

Milestone 13 is complete in the linked application repo.

It added live Alpaca paper-trading integration behind the accepted broker execution boundary while keeping broker workflows core-services and CLI-only.

## Implemented Shape

Milestone 13 added:

- `alpaca-py`
- `AlpacaPaperBrokerClient`
- paper-only Alpaca client construction
- vault-first, environment-fallback credential resolution for `ALPACA_API_KEY` and `ALPACA_SECRET_KEY`
- CLI provider selection with `submit-paper-order --provider alpaca`
- Alpaca sync through `sync-paper-order <broker-order-id>` without simulated fill price
- preservation of simulated paper broker behavior
- fake-client Alpaca adapter tests without live network calls

The implementation maps local `OrderIntent` records to Alpaca paper order requests and maps Alpaca order status back into local `BrokerOrderStatus`.

## Boundary

Milestone 13 does not add:

- real-money execution
- FastAPI broker endpoints
- React broker controls
- browser execution buttons
- autonomous trading
- broker-position reconciliation
- full order-management-system behavior

Broker data remains external execution fact. Local JSON remains the source of truth for internal trade records and audit history.

## Validation

Recorded on 2026-05-03:

```text
uv run pytest tests\test_alpaca_paper_broker.py tests\test_broker_execution_service.py tests\test_cli_workflow_commands.py
44 passed

uv run pytest
280 passed
```

The only warning recorded in the raw note was a third-party `websockets.legacy` deprecation warning from the Alpaca dependency path.

## Related Pages

- [[milestone-13-alpaca-paper-adapter]]
- [[milestone-12-paper-execution-hardening]]
- [[milestone-11-broker-boundary-and-paper-trading]]
- [[post-milestone-11-roadmap]]
