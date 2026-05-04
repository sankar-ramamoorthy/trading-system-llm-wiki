---
title: High-Level Milestone 17 And Milestone 18
type: processed-note
status: processed
tags: [trading-system, milestone-17, milestone-18, broker-boundary, paper-trading, web]
created: 2026-05-04
processed: 2026-05-04
source: knowledge/raw/high level milestone 17 and milestone 18.md
---

# High-Level Milestone 17 And Milestone 18

## Summary

Milestones 17 and 18 move the broker workflow from CLI-only toward the browser in two separate steps:

- Milestone 17 is read-only API/web broker visibility.
- Milestone 18 is human-controlled browser paper execution controls.

The split matters because the browser should first prove it can display broker state clearly before it is allowed to mutate broker state.

## Milestone 17: Read-Only API/Web Broker Visibility

Milestone 17 should expose broker execution facts in FastAPI and React without allowing browser broker actions.

Current broker workflows mostly exist through CLI commands:

```text
submit-paper-order
sync-paper-order
sync-broker-orders
reconcile-broker-orders
list-broker-orders
show-broker-order
```

Milestone 17 should expose the read side of that state in the browser:

- broker order status: submitted, filled, canceled, rejected
- provider: simulated or Alpaca
- provider order id
- linked `OrderIntent`
- linked `Position`
- linked fills
- fill price and quantity
- submitted and synced timestamps
- mismatch or reconciliation information from Alpaca vs local state

The point is operational awareness. The user can inspect what happened without remembering CLI commands, but cannot submit, sync, cancel, reconcile, or otherwise mutate broker state from the browser.

## Milestone 18: Human-Controlled Browser Paper Execution Controls

Milestone 18 should add browser controls for paper execution actions after read-only visibility is stable.

Likely browser actions:

- submit a paper order from an existing approved local `OrderIntent`
- choose provider, likely simulated or Alpaca
- sync a submitted broker order
- possibly run batch sync or reconciliation later, if explicitly accepted
- show a confirmation screen before submission

The controls must stay human-controlled. The browser should not generate trades, recommend trades, auto-submit, schedule orders, or trade real money. It should provide a safer UI around manual paper-trading actions that already exist in the CLI.

## Likely Milestone 18 Flow

```text
open trade plan or position in browser
show approved OrderIntent
click paper-submit action
show confirmation with symbol, side, quantity, order type, limit/stop prices, provider, linked position, and risk context
confirm
submit paper order and create/update local BrokerOrder records
later sync broker status/fills back into local records
```

## Boundary

The durable boundary across both milestones:

- `BrokerOrder` and `Fill` stay the local audit records.
- Broker data remains external execution fact.
- Browser controls should not create trade meaning.
- Browser controls should not create recommendations.
- Real-money execution remains outside these milestones.
- Automated execution remains outside these milestones.

## Promoted Topics

- [[milestone-17-api-web-broker-visibility]]
- [[milestone-18-browser-paper-execution-controls]]
