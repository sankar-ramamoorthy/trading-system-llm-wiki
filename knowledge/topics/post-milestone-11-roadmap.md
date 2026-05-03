---
title: Post-Milestone 11 Roadmap
type: topic
status: draft
tags: [trading-system, roadmap, broker-boundary, paper-trading, alpaca, real-money-readiness]
created: 2026-05-03
updated: 2026-05-03
---

# Post-Milestone 11 Roadmap

Milestone 11 closed the first broker boundary with simulated paper execution through core services and CLI only.

The next broker-related work should move in controlled layers:

```text
M12 paper execution hardening - complete
M13 Alpaca paper adapter - complete
M14 broker reconciliation and status sync - next candidate
M15 API/web broker visibility
M16 browser paper execution controls
real-money readiness gate
```

This sequence is partially implemented. Milestones 12 and 13 are complete in the application repo; Milestones 14 through 16 remain proposed staged direction until accepted through issue maps.

## Current Boundary

Milestone 11 established:

- ADR-011 Broker Execution Boundary
- provider-agnostic broker execution port
- simulated paper broker adapter
- local `BrokerOrder` records
- broker-linked fills
- CLI commands: `submit-paper-order`, `sync-paper-order`, `show-broker-order`

It deliberately did not add:

- live Alpaca calls
- FastAPI broker endpoints
- React broker controls
- real-money execution
- autonomous trading
- recommendations
- full OMS behavior

## Milestone 12 Complete: Paper Execution Hardening

Milestone 12 made the simulated paper workflow easier to inspect and safer to operate before any external broker was added.

Implemented direction:

- broker-order list/read models
- clearer broker-order links in plan, position, and timeline views
- lifecycle audit improvements for submitted, filled, canceled, rejected, and repeated sync cases
- simulated cancellation/rejection support
- CLI/core-services scope only

Milestone 12 did not add Alpaca, API broker endpoints, or React broker controls.

## Milestone 13 Complete: Alpaca Paper Adapter

Milestone 13 added live Alpaca paper trading behind the existing broker port.

Implemented direction:

- Alpaca adapter implementing the accepted `BrokerClient` boundary
- vault-first, environment-fallback credentials
- reserved secret names: `ALPACA_API_KEY`, `ALPACA_SECRET_KEY`
- paper submission only from existing approved local `OrderIntent` plus open local `Position`
- Alpaca order/fill state imported into local `BrokerOrder` and `Fill`
- real-money execution blocked
- CLI-only controls: `submit-paper-order --provider alpaca` and `sync-paper-order`

Alpaca should conform to the local boundary; local domain records should not reshape themselves around Alpaca response objects.

## Milestone 14 Candidate: Broker Reconciliation And Sync

Milestone 14 should handle local-vs-broker mismatches explicitly.

Likely direction:

- broker order sync/reconciliation commands
- clear mismatch reporting
- idempotent fill import
- audit events for sync results and mismatches
- broker positions remain external facts, not canonical local `Position` records

The system should surface mismatch instead of silently correcting trade meaning.

## Milestone 15 Candidate: API/Web Broker Visibility

Milestone 15 should expose broker order visibility without browser execution controls.

Likely direction:

- read-only FastAPI endpoints for broker orders
- broker order status and linked fills in web plan/position views
- no browser submit/sync/cancel actions
- no autonomous execution or recommendations

## Milestone 16 Candidate: Browser Paper Execution Controls

Milestone 16 can add human-controlled browser paper execution after CLI and API behavior are proven.

Likely direction:

- explicit submit/sync controls in React
- confirmation before paper submission
- display linked order intent, position, provider, quantity, side, and order type before submit
- continued block on real-money execution
- no generated recommendations or autonomous behavior

## Real-Money Gate

Real-money execution is not a default numbered milestone.

It requires a readiness gate with evidence:

- stable paper trading behavior
- clean local records
- consistent review discipline
- known playbook/invalidation quality
- useful reconciliation behavior
- accepted ADR for real-money boundaries and failure modes

Until then, all broker work remains paper-only and human-controlled.

## Source Notes

- [[post-milestone-11-higher-level-roadmap-20260503]]
- [[milestone-12-paper-execution-hardening]]
- [[milestone-13-alpaca-paper-adapter]]
- [[milestone-11-broker-boundary-and-paper-trading]]
- Application repo `STATUS.md`
- Application repo `PROJECT.md`
- Application repo `DOCS/ADR/011-broker-execution-boundary.md`
- Application repo `DOCS/milestone-11-issue-map.md`
- Application repo `DOCS/milestone-12-issue-map.md`
- Application repo `DOCS/milestone-13-issue-map.md`
