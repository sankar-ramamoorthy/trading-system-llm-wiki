---
title: Post-Milestone 11 Higher-Level Roadmap
type: processed-note
status: processed
tags: [trading-system, roadmap, milestone-11, milestone-12, broker-boundary, paper-trading, alpaca]
created: 2026-05-03
---

# Post-Milestone 11 Higher-Level Roadmap

This note processes the raw note `Post-Milestone 11 Higher-Level Roadmap.md`.

## Status

Milestone 11 is complete in the linked application repo. It added ADR-011, a provider-agnostic broker execution port, local `BrokerOrder` persistence, simulated paper broker execution, broker-linked fills, and CLI-only paper execution commands.

Recorded validation from the application repo:

```text
Focused broker/order-intent/fill/persistence/CLI tests: 71 passed
Full suite: 257 passed
```

The application repo `DOCS/product-roadmap.md` still contains stale wording that calls Milestone 11 the next planned slice. Current `STATUS.md`, `PROJECT.md`, ADR-011, and `DOCS/milestone-11-issue-map.md` are better sources for current state.

## Durable Principle

Broker data remains external execution fact. Local trade records remain the system's source of truth for trade meaning, lifecycle audit, reviews, and decision history.

All broker expansion should stay human-controlled, paper-only, and explicit until a later readiness gate changes that boundary through a new ADR.

## Recommended Post-M11 Sequence

### Milestone 12: Paper Execution Hardening

Status: complete in the application repo.

Goal: make the simulated paper execution workflow easier to inspect, safer to operate, and harder to misuse.

Likely scope:

- better broker-order read models and listing commands
- clearer broker-order links in position, plan, and timeline views
- stronger lifecycle audit around submitted, filled, canceled, rejected, and repeated sync cases
- cancellation/rejection support for simulated paper orders if needed
- core services and CLI only

Milestone 12 did not add Alpaca, FastAPI broker endpoints, or React broker controls.

### Milestone 13: Alpaca Paper Adapter

Status: complete in the application repo.

Goal: add live Alpaca paper-trading integration behind the existing broker port.

Likely scope:

- Alpaca paper adapter using `BrokerClient`
- reserved credential names `ALPACA_API_KEY` and `ALPACA_SECRET_KEY`
- vault-first, environment-fallback credential resolution
- paper order submission only from an existing approved local `OrderIntent` plus open local `Position`
- Alpaca order/fill status imported into local `BrokerOrder` and `Fill` records
- real-money execution explicitly blocked
- controls stay CLI-only unless a later milestone decides otherwise

Alpaca fits the accepted broker boundary rather than reshaping local domain records around Alpaca-specific API models.

### Milestone 14: Broker Reconciliation And Status Sync

Goal: handle mismatch between local records and broker-side facts.

Likely scope:

- explicit sync/reconciliation commands for broker orders
- detection of broker order status changes without silently mutating trade meaning
- clear local-vs-broker mismatch reports
- idempotent fill import
- audit events for sync results and mismatches
- broker positions remain non-canonical relative to local `Position` records

### Milestone 15: API/Web Broker Visibility

Goal: expose broker order visibility through API and browser views without adding browser execution buttons yet.

Likely scope:

- read-only FastAPI endpoints for broker orders
- broker order status and linked fill display in web plan/position views
- no browser submit/sync/cancel actions
- no autonomous execution or recommendations

Browser visibility is lower risk than browser execution and can validate the operator model before web controls exist.

### Milestone 16: Browser Paper Execution Controls

Goal: add human-controlled browser paper execution after CLI and API behavior are proven.

Likely scope:

- explicit submit/sync controls in the web UI
- clear confirmation before paper submission
- display linked order intent, position, provider, quantity, side, and order type before submit
- continued real-money execution block
- no generated recommendations or autonomous behavior

Browser execution controls have higher blast radius than CLI commands and should follow proven service behavior.

## Deferred Boundaries

These remain deferred until explicitly accepted:

- live Alpaca paper submission: implemented in Milestone 13 for paper-only CLI workflows
- FastAPI broker controls: Milestone 15 or later candidate
- React broker controls: Milestone 16 or later candidate
- real-money trading: readiness gate, not a default numbered milestone
- autonomous trading: out of scope
- recommendations or AI-generated execution instructions: out of scope
- full OMS behavior: out of scope unless a future ADR changes the product boundary

## Real-Money Readiness Gate

Real-money execution should not be planned as the automatic next milestone after browser paper controls.

It requires evidence:

- stable paper trading behavior
- clean local records
- consistent review discipline
- known playbook and invalidation quality
- useful reconciliation behavior
- explicit ADR accepting real-money boundaries and failure modes

Until that gate is passed, broker work remains paper-only and human-controlled.

## Related Pages

- [[post-milestone-11-roadmap]]
- [[milestone-12-paper-execution-hardening]]
- [[milestone-13-alpaca-paper-adapter]]
- [[milestone-11-broker-boundary-and-paper-trading]]
- [[product-roadmap-and-learning-boundaries]]
- [[data-and-platform-strategy]]
- [[deterministic-rules-vs-contextual-intelligence]]
