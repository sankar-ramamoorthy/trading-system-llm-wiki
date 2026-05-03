---
title: Proposed Milestone 11 Plan
type: raw-note
status: processed
tags: [trading-system, milestone-11, broker-boundary, paper-trading]
created: 2026-05-03
processed: 2026-05-03
processed_to:
  - knowledge/processed/proposed-milestone-11-broker-boundary-paper-execution-20260503.md
  - knowledge/topics/milestone-11-broker-boundary-and-paper-trading.md
---

 For CLI + API + web in one milestone: yes, it can be done as separate sequential issues, but it changes Milestone 11 from “broker boundary proof” into a
  product workflow milestone. The likely shape would be:

  - 11A ADR: execution boundary, source of truth, allowed order types, no real money.
  - 11B Core service + fake broker adapter: submit approved OrderIntent, record broker order state, import fake broker fills.
  - 11C Alpaca paper adapter: real provider wiring behind the same port, using vault/env credentials.
  - 11D CLI commands: operator workflow for submit/check/import.
  - 11E FastAPI endpoints: same actions exposed to local API.
  - 11F Web UI: buttons/status panels in plan detail for order intents and paper broker status.
  - 11G Closeout: docs/tests/manual validation.

  That is coherent, but the risk is that API/web polish will distract from getting the broker boundary correct. My engineering recommendation is Milestone
  11 = ADR + core + CLI + fake adapter + maybe live Alpaca adapter, then Milestone 12 = browser execution workbench. That keeps paper trading human-
  controlled and auditable without turning the first broker slice into a web product expansion.

  For position handling, the issue is that today fills require a position_id, and positions are opened explicitly from approved plans. Broker fills come
  back linked to broker orders, not to your local position automatically. The choices mean:

  - Require position first: workflow is approve plan -> evaluate rules -> create OrderIntent -> open local position -> submit paper order -> import fills
    into that position. This is safest with today’s model. It preserves “no position without local intent” and does not let the broker create internal
    meaning.
  - Auto-open on first fill: workflow is OrderIntent -> submit paper order -> fill arrives -> system opens local position from the plan and records fill.
    This is ergonomic and probably correct eventually, but it means broker execution can trigger local lifecycle creation. Still human-controlled, but more
    design surface.
  - Open on submit: workflow is submit paper order -> system opens local position immediately. I do not recommend this because an order can be rejected,
    canceled, or unfilled. It creates internal position state before execution fact exists.

  My recommendation: Require position first for Milestone 11, and record “auto-open on broker fill” as a later improvement once broker-order/fill mapping is
  proven.

• Questions 2/2 answered
  • For Milestone 11, which surface scope should the plan use?
    answer: Core + CLI (Recommended)
  • For Milestone 11, how should imported broker fills attach to local position state?
    answer: Require position (Recommended)
	
 Proposed Plan


  # Milestone 11 Plan: Broker Boundary And Simulated Paper Execution

  ## Summary

  Milestone 11 starts with ADR-011: Broker Execution Boundary and implements a narrow, human-controlled broker boundary before live Alpaca calls.

  Decisions locked for this plan:

  - Surface: core services + CLI only. No FastAPI or React paper-trading controls in Milestone 11.
  - Position handling: require an existing local position before broker fills can be imported.
  - Adapter depth: simulated paper broker + Alpaca-ready port. Live Alpaca paper submission is deferred to a later milestone/issue.
  - Source of truth: broker data is external execution fact; local JSON remains the source for internal trade records and lifecycle audit.

  ## Key Changes

  - Add DOCS/ADR/011-broker-execution-boundary.md.
      - Accept provider-agnostic broker port.
      - Accept paper-only scope.
      - Explicitly reject real-money execution, autonomous trading, recommendations, and full OMS behavior.
      - Reserve future Alpaca credential names: ALPACA_API_KEY, ALPACA_SECRET_KEY.
      - State that future Alpaca integration should use vault-first, environment-fallback resolution.
  - Add DOCS/milestone-11-issue-map.md.
      - 11A: ADR.
      - 11B: broker order domain/persistence boundary.
      - 11C: simulated paper broker adapter + execution service.
      - 11D: CLI commands.
      - 11E: closeout docs/status/validation.
  - Add a local BrokerOrder domain record.
      - Fields: local id, order_intent_id, position_id, provider, provider order id, symbol, side, order type, quantity, optional limit/stop prices,
        status, submitted timestamp, updated timestamp.
      - Status values: submitted, filled, canceled, rejected.
      - One OrderIntent may have only one broker order in Milestone 11.
  - Extend Fill with optional broker_order_id.
      - Existing manual fills continue to work.
      - Old JSON records load with broker_order_id = None.
      - Broker-imported fills use source = "broker:simulated".
  - Add broker repository support.
      - Add BrokerOrderRepository protocol.
      - Add JSON and in-memory implementations.
      - Add broker_orders collection to the local JSON store.
  - Add broker port and simulated adapter.
      - BrokerClient port supports submit and sync/status retrieval.
      - SimulatedPaperBrokerClient returns deterministic paper results.
      - Sync requires a simulated fill price for Milestone 11 so tests and demos are explicit.
  - Add execution service.
      - submit_paper_order(order_intent_id, position_id, provider="simulated")
          - rejects missing/canceled OrderIntent
          - rejects missing/closed position
          - rejects position/order-intent trade-plan mismatch
          - rejects duplicate broker order for the same OrderIntent
          - stores BrokerOrder
          - emits BROKER_ORDER_SUBMITTED
      - sync_paper_order(broker_order_id, simulated_fill_price)
          - updates broker order status to filled
          - records one local fill linked to order_intent_id and broker_order_id
          - updates the existing position through fill logic
          - emits BROKER_ORDER_FILLED and existing fill/position lifecycle events
          - is idempotent: repeated sync does not create duplicate fills
  - Add CLI commands.
      - show-broker-order <broker-order-id>
      - No API or web controls in this milestone.

  ## Test Plan

  - ADR/docs checks are review-based.
  - Unit tests:
      - cannot submit missing/canceled order intent
      - cannot submit against missing/closed/mismatched position
      - cannot submit duplicate broker order for one order intent
      - successful submit stores BrokerOrder and lifecycle event
      - sync records one broker-sourced fill and updates position quantity
      - repeated sync is idempotent
      - broker fill can close a position through existing position logic
      - JSON persistence round-trips BrokerOrder and old fills without broker_order_id
  - CLI tests:
      - submit-paper-order prints broker order id/status
      - sync-paper-order prints fill id, broker order id, position state, open quantity
      - show-broker-order displays metadata without secrets
  - Regression:
      - focused broker/order-intent/fill/persistence/CLI tests
      - full uv run pytest

  ## Assumptions

  - Milestone 11 does not install alpaca-py or make live Alpaca API calls.
  - Future live Alpaca work will use Alpaca’s paper trading client with paper=True and submit_order; official references checked:
      - https://alpaca.markets/sdks/python/trading.html
      - https://alpaca.markets/sdks/python/api_reference/trading/trading-client.html
      - https://alpaca.markets/sdks/python/api_reference/trading/orders.html
  - Future browser execution belongs in a later milestone after the broker boundary is proven through CLI.

