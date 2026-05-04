---
title: Milestone 18 Browser Paper Execution Controls
type: topic
status: proposed
tags: [trading-system, milestone-18, broker-boundary, paper-trading, web]
created: 2026-05-04
updated: 2026-05-04
---

# Milestone 18 Browser Paper Execution Controls

Milestone 18 is proposed as human-controlled browser controls for paper execution.

It should happen after Milestone 17 proves read-only broker visibility in the browser.

## Intent

Milestone 18 should add a safer UI around paper-trading actions that already exist manually through the CLI.

Likely browser actions:

- submit a paper order from an existing approved local `OrderIntent`
- choose provider, likely simulated or Alpaca
- sync a submitted broker order
- possibly run batch sync or reconciliation later, if explicitly accepted
- show a confirmation screen before submission

The key boundary is human control. The browser may execute a user-confirmed paper action, but it must not generate trades, recommend trades, auto-submit, schedule orders, or trade real money.

## Likely Flow

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

Milestone 18 should not add:

- real-money execution
- automated execution
- scheduled orders
- AI-generated execution instructions
- trade recommendations
- browser-created trade meaning
- broker-driven local position creation outside accepted local workflow rules

`BrokerOrder` and `Fill` remain local audit records. Broker data remains external execution fact. Browser controls should only operate on existing local trade intent and should preserve the local source-of-truth boundary.

## Related Pages

- [[high-level-milestone-17-and-milestone-18-20260504]]
- [[milestone-17-api-web-broker-visibility]]
- [[milestone-14-broker-reconciliation-and-status-sync]]
- [[milestone-11-broker-boundary-and-paper-trading]]
- [[post-milestone-11-roadmap]]
