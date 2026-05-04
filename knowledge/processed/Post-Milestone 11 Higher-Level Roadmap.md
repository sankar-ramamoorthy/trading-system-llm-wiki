---
title: Post-Milestone 11 Higher-Level Roadmap
type: raw-note
status: processed
tags: [trading-system, roadmap, milestone-11, broker-boundary, paper-trading]
created: 2026-05-03
processed: 2026-05-03
processed_to:
  - knowledge/processed/post-milestone-11-higher-level-roadmap-20260503.md
  - knowledge/topics/post-milestone-11-roadmap.md
  - knowledge/topics/milestone-11-broker-boundary-and-paper-trading.md
---

 # Post-Milestone 11 Higher-Level Roadmap

  ## Summary

  Milestone 11 closed the first broker boundary, but it deliberately stopped short of real broker connectivity and browser execution controls. The next
  work should move in controlled layers: first prove the CLI paper workflow, then add a real paper broker adapter, then expose broker workflows to API/web
  only after the core behavior is stable.

  The main principle remains: broker data is external execution fact; local trade records remain the system’s source of truth.

  ## Recommended Next Milestones

  ### Milestone 12: Paper Execution Hardening

  Goal: make the current simulated paper execution workflow easier to inspect, safer to operate, and harder to misuse.

  Key direction:

  - Add better broker-order read models and listing commands.
  - Surface broker order links in position, plan, and timeline views more clearly.
  - Improve lifecycle audit around submitted, filled, canceled, rejected, and repeated sync cases.
  - Add cancellation/rejection support for simulated paper orders if needed.
  - Keep scope CLI/core services only.
  - Do not add Alpaca, FastAPI broker endpoints, or React broker controls yet.

  Why this comes next: Milestone 11 created the boundary; Milestone 12 should prove the local model and operator workflow before introducing an external
  broker.

  ### Milestone 13: Alpaca Paper Adapter

  Goal: add live Alpaca paper-trading integration behind the existing broker port.

  Key direction:

  - Add Alpaca paper adapter using the existing BrokerClient port.
  - Use reserved credential names: ALPACA_API_KEY, ALPACA_SECRET_KEY.
  - Resolve credentials vault-first, environment-fallback.
  - Submit paper orders only from existing approved local OrderIntent plus open local Position.
  - Import Alpaca order/fill status into local BrokerOrder and Fill records.
  - Keep real-money execution explicitly blocked.
  - Keep controls CLI-only unless a later milestone decides otherwise.

  Why this comes after hardening: Alpaca should fit the existing boundary, not reshape the domain around Alpaca’s API.

  ### Milestone 14: Broker Reconciliation And Status Sync

  Goal: handle mismatch between local records and broker-side facts.

  Key direction:

  - Add explicit sync/reconciliation commands for broker orders.
  - Detect broker order status changes without silently mutating trade meaning.
  - Report local-vs-broker mismatches clearly.
  - Preserve idempotent fill import.
  - Record audit events for sync results and mismatches.
  - Do not treat broker positions as canonical system positions.

  Why this matters: once Alpaca exists, the system needs a disciplined answer to “broker says X, local system says Y.”

  ### Milestone 15: API/Web Broker Visibility

  Goal: expose broker order visibility in FastAPI and React without adding browser execution buttons yet.

  Key direction:

  - Add read-only API endpoints for broker orders.
  - Show broker order status and linked fills in the web plan/position views.
  - Keep submission/sync/cancel actions out of the browser at first.
  - Do not add autonomous execution or recommendations.

  Why this comes later: browser visibility is lower risk than browser execution, and it helps validate the operator model.

  ### Milestone 16: Browser Paper Execution Controls

  Goal: add human-controlled browser paper execution after CLI and API behavior are proven.

  Key direction:

  - Add explicit submit/sync controls in the web UI.
  - Require clear confirmation before paper submission.
  - Display linked order intent, position, provider, quantity, side, and order type before submit.
  - Continue to block real-money execution.
  - Continue to avoid generated recommendations or autonomous behavior.

  Why this is delayed: execution controls in the browser are higher blast-radius than CLI commands and should follow proven service behavior.

  These were intentionally postponed and should remain deferred until their milestone:

  - Live Alpaca paper submission: Milestone 13.
  - FastAPI broker controls: Milestone 15 or later.
  - React broker controls: Milestone 16 or later.
  - Real-money trading: not a normal milestone; it requires a readiness gate.
  - Autonomous trading: still out of scope.
  - Recommendations or AI-generated execution instructions: still out of scope.
  - Full OMS behavior: out of scope unless a future ADR changes the product boundary.

  ## Real-Money Readiness Gate

  Real-money execution should not be planned as “Milestone 17” by default. It should be a gate that requires evidence:

  - stable paper trading behavior
  - clean local records
  - consistent review discipline
  - known playbook/invalidation quality
  - useful reconciliation behavior
  - explicit ADR accepting real-money boundaries and failure modes

  Until then, all broker work remains paper-only and human-controlled.

  ## Assumptions

  - The next practical milestone should be Milestone 12: paper execution hardening.
  - Alpaca should not be added until the current simulated workflow is easier to inspect and reconcile.
  - Browser execution should wait until after CLI plus API read visibility are stable.
  - The stale roadmap text that called Milestone 11 “next planned” should be updated in a future docs cleanup, but current STATUS.md, PROJECT.md, ADR-011,
    and the Milestone 11 issue map are the better source of truth.
