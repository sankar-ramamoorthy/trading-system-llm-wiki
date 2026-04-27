---
title: API-First Trade Capture Product Vision
type: topic
status: draft
tags: [trading-system, product-vision, api-first, web-ui, trade-capture, trader-language-input]
created: 2026-04-27
---

# API-First Trade Capture Product Vision

The trading system is evolving from a command-line discipline and journaling tool into a local trade workflow product.

The near-term goal is not automation. The goal is to make trade capture fast, structured, and natural enough that a trader can use it during real decision-making without fighting the interface.

## Product Direction

The intended product surface is an API-first local web app:

- FastAPI backend over the existing trading-system services
- React/Vite frontend for the primary user experience
- existing CLI retained for power use, debugging, scripts, and administration
- local JSON persistence retained for the first product stage

The first web workflow should focus on capturing a trade thought and turning it into structured drafts.

## The User Problem

A trader does not naturally think in UUIDs or database fields.

They think in language like:

```text
I may take a long swing trade in NVDA using my pullback-to-trend playbook over a days-to-weeks horizon.
Entry: buy at X.
Stop: sell below Y.
Target: take profit at Z.
NVDA is holding the 20DMA, sector leadership is intact, and volume dried up on the pullback.
```

The product should understand that this contains several different kinds of information.

## What The Product Should Extract

The first workflow should turn the user's draft into three editable sections.

`TradeIdea` captures what the possible trade is:

```text
symbol: NVDA
direction: long
purpose: swing
horizon: days_to_weeks
playbook: pullback-to-trend
```

`TradeThesis` captures why the trade might work:

```text
reasoning: NVDA is holding the 20DMA, sector leadership is intact, and volume dried up on the pullback.
```

`TradePlan` captures how the trade would be executed:

```text
entry_criteria: buy at X
invalidation: sell below Y
targets:
  - take profit at Z
```

These should be drafts first. The user should be able to edit the fields before anything is saved.

## First Workflow

The first web screen should behave like a focused capture workspace:

- the user types or pastes a rough trade thought
- the system parses it into draft `TradeIdea`, `TradeThesis`, and `TradePlan` sections
- the UI shows missing or ambiguous fields clearly
- the user edits the drafts
- the user explicitly saves when satisfied
- the system creates linked domain records only after that explicit save

This preserves the current discipline model while making the experience easier to use.

## What The Early Version Does Not Do

The early version should not:

- suggest trades
- recommend buy, stop, or target levels
- verify whether market claims are true
- approve plans
- create order intents
- open positions
- record fills
- execute orders

The early version only translates user-authored trade language into editable structured drafts.

## Future Direction

Later versions may add thesis verification.

For example, the claim:

```text
NVDA is holding the 20DMA, sector leadership is intact, and volume dried up on the pullback.
```

could eventually be decomposed into testable claims:

- is NVDA above or near its 20-day moving average?
- is volume below recent averages during the pullback?
- is the relevant sector or peer group still showing leadership?

The system could then return evidence, confidence, data timestamps, and an assessment such as supported, contradicted, unclear, or insufficient data.

Later versions may also suggest candidate entry, stop, or target levels. That should be treated as decision support, not automatic trading intent. The user must remain responsible for accepting, editing, or rejecting those suggestions.

## Product Principle

The product should let the trader think in normal trading language while the system maintains structured, auditable records.

The sequence is:

```text
natural trader language
-> editable structured drafts
-> explicit user save
-> auditable trade lifecycle
```

That boundary keeps the system useful without turning early parsing into unsafe automation.

## Related Pages

- [[trade-lifecycle-and-objects]]
- [[canonical-domain-model]]
- [[context-intelligence-layer]]
- [[deterministic-rules-vs-contextual-intelligence]]
- [[product-roadmap-and-learning-boundaries]]
- [[brainstorm-20260427-api-first-web-product-and-trader-language-input]]
