---
title: Milestone 13 Alpaca Paper Adapter
type: topic
status: complete
tags: [trading-system, milestone-13, alpaca, paper-trading, broker-boundary, cli]
created: 2026-05-03
updated: 2026-05-03
---

# Milestone 13 Alpaca Paper Adapter

Milestone 13 is complete in the linked application repo.

It added Alpaca paper trading behind the existing broker port while preserving the Milestone 11 execution boundary. The implementation remains core services plus CLI only.

## Implemented Shape

Milestone 13 added:

- official `alpaca-py` dependency
- `AlpacaPaperBrokerClient`
- paper-only `TradingClient(..., paper=True)`
- vault-first, environment-fallback credentials for `ALPACA_API_KEY` and `ALPACA_SECRET_KEY`
- local `OrderIntent` mapping to Alpaca market, limit, stop, and stop-limit requests
- Alpaca status mapping into local `BrokerOrderStatus`
- `submit-paper-order --provider alpaca`
- provider-based `sync-paper-order`
- Alpaca sync without `--simulated-fill-price`
- preservation of simulated sync with explicit fill price
- non-filled provider sync that updates local `BrokerOrder` without importing a fill

Tests use fake clients and do not make live network calls.

## Boundary

Milestone 13 did not add:

- real-money execution
- FastAPI broker endpoints
- React broker controls
- browser execution buttons
- autonomous trading
- broker-position reconciliation
- full OMS behavior

Broker data remains external execution fact. Local JSON remains the source of truth for internal trade records and audit history.

## Validation

Recorded on 2026-05-03:

```text
uv run pytest tests\test_alpaca_paper_broker.py tests\test_broker_execution_service.py tests\test_cli_workflow_commands.py
44 passed

uv run pytest
280 passed
```

## Related Pages

- [[implemented-milestone-13-alpaca-paper-adapter-20260503]]
- [[milestone-12-paper-execution-hardening]]
- [[milestone-11-broker-boundary-and-paper-trading]]
- [[post-milestone-11-roadmap]]
- [[milestone-10-secure-credentials]]
