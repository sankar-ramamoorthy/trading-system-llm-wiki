---
title: System Capability After Milestone 7
type: processed
status: active
tags: [trading-system, capability, elevator-pitch, milestone-7, baseline]
created: 2026-05-02
updated: 2026-05-02
---

# System Capability After Milestone 7

Processed from raw note: `Elevator pitch and system capability after mileston 7.md`

This is the canonical capability baseline as of Milestone 7 completion (2026-05-02). Use it to orient new sessions and to track what the system can do before planning new milestones.

---

## Elevator Pitch

> A local, structured trading workflow system for a single disciplined discretionary trader. You capture trade ideas in plain language — the system parses them into structured plans, enforces your own rules before you can act, records every fill manually, tracks positions automatically, and builds a reviewable journal of every trade you've taken. No automation, no broker wiring, no black boxes. Just structure, discipline, and an honest record of your decisions.

---

## Two Entry Points

**1. Web interface** (`docker compose up`, open `http://localhost:5173`)

Enter a raw trade note in plain language → LLM parses it into a structured `TradeIdea`/`TradeThesis`/`TradePlan` draft → review and edit any flagged fields → explicit Save creates linked records in the local store.

**2. CLI** (`uv run trading-system <command>`)

The full trade lifecycle from first idea to post-trade review, all via structured commands.

---

## Full Capability Inventory

| Area | What it does |
|---|---|
| Trade capture (web) | Natural-language → parse → edit → save via browser UI; Groq or Ollama LLM backends |
| Trade lifecycle (CLI) | `TradeIdea → TradeThesis → TradePlan → approve → rule eval → OrderIntent → Position → Fill → close` |
| Discipline enforcement | Deterministic rule evaluation before a plan can proceed to execution |
| Position tracking | Auto-updates from fills; auto-closes when quantity reaches zero |
| P&L | Read-side realized P&L for closed positions |
| Trade review | Structured post-trade review with tags, 1–5 quality scores per dimension |
| Journal export | Filtered Markdown export of reviewed trades |
| Market context | Explicit daily OHLCV snapshots via yfinance or Massive.com; advisory only, non-canonical |
| Context attachment | Snapshots can be linked to plans, positions, or reviews |
| Local JSON store | Single-file persistence; validate, backup, and restore commands |
| Audit trail | `LifecycleEvent` records every state transition throughout the workflow |
| Reference data | Seeded instrument symbols and playbook slugs for lookup |
| API | FastAPI backend exposing health, reference lookup, parse, save, and saved-result retrieval |

---

## What It Deliberately Does Not Do

- Broker integration
- Automated execution
- Plan approval from the web (web captures only; approval remains CLI)
- Live market data
- AI-generated advice or recommendations
- Cloud deployment
- Multi-user access

---

## Milestone 8+ Direction

This baseline is the foundation for the near-term roadmap:

- **Milestone 8**: Options chain ingestion (yfinance + Massive.com)
- **Milestone 9**: Web product beyond first capture (list/detail views, plan approval, context attachment)
- **Milestone 10**: Secure credentials (local key vault)
- **Milestone 11**: Broker boundary and paper trading (Alpaca)

See `DOCS/product-roadmap.md` for the accepted near-term sequence.
