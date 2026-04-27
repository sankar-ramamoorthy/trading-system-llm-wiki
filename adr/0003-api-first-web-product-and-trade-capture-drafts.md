---
title: API-First Web Product And Trade Capture Drafts
type: adr
status: accepted
tags: [trading-system, architecture, api-first, web-ui, trade-capture, trader-language-input]
created: 2026-04-27
---

# ADR 0003 - API-First Web Product And Trade Capture Drafts

## Context

The trading system has proven a local-first manual workflow through the CLI, including `TradeIdea`, `TradeThesis`, `TradePlan`, plan approval, rule evaluation, order intent, position lifecycle, fills, and review.

The authoritative application-repo decision record is:

```text
C:\Users\bosto\dockerstuff\trading-system\DOCS\ADR\008-api-first-web-product-and-trade-capture-drafts.md
```

This knowledge-base ADR preserves the product reasoning and cross-links for the wiki. The application repo ADR is the source of truth for runtime architecture.

The CLI remains useful, but recent usage exposed a product-level limitation: the user must currently provide implementation-facing identifiers such as `instrument_id` and `playbook_id`. A trader naturally thinks in symbols, playbook names, thesis language, entry/stop/target notes, and evidence. Requiring UUIDs or multiple command invocations makes the system harder to use during real trade capture.

The product direction now requires a user-facing workflow that can accept normal trader language and turn it into editable structured drafts without weakening the existing domain boundaries.

## Decision

The next product direction is an API-first local web product.

Accepted architecture decisions:

- Add a local FastAPI service boundary over the existing domain and service layers.
- Use a React/Vite web UI as the intended primary user-facing product surface.
- Keep the existing Typer CLI as a supported power, debug, admin, and scripting surface.
- Keep the current local JSON store as the active persistence backend for this stage.
- Add user-friendly lookup support so workflows can resolve symbols and playbook names instead of requiring UUIDs in user-facing input.
- Make the first web workflow a trade-capture draft workflow.
- Let the user enter normal trader language and receive editable draft sections for:
  - `TradeIdea`: what the trade is
  - `TradeThesis`: why it might work
  - `TradePlan`: how it would be executed
- Do not persist parsed output until the user explicitly saves.
- Add a trader-language parser port with a deterministic or stub implementation first.
- Allow future LLM parsing behind the same parser port without changing the API or persistence workflow.

The first parser is extraction-only. It must not suggest trades, verify market claims, recommend entry or stop levels, approve plans, create order intents, open positions, record fills, or execute orders.

## Consequences

### Positive

- The product can support trader-native workflows without discarding the existing domain model.
- The API boundary can support both the web UI and future clients while preserving the CLI.
- Draft review before save protects auditability and avoids silent parser mistakes.
- The parser port allows incremental implementation before adding a real LLM dependency.
- Keeping JSON persistence avoids coupling this product step to a storage migration.

### Negative

- The application gains a new runtime surface and dependency set.
- API schemas and frontend state become additional contracts to maintain.
- Instrument and playbook lookup semantics must be made more explicit.
- The parser can create false confidence if ambiguity and missing fields are not surfaced clearly.
- A web UI introduces design and usability work that the CLI previously avoided.

## Deferred

This ADR explicitly defers:

- broker integration
- Postgres migration
- production auth, cloud deployment, or multi-user hosting
- trade suggestions
- system-generated buy, stop, or target recommendations
- claim verification such as checking whether a symbol is holding the 20DMA
- plan approval, order intent creation, position opening, or execution from the capture screen
- replacing or removing the CLI

Future claim verification belongs in a context-intelligence or thesis-verification layer. Future system suggestions belong in a separate decision-support boundary with explicit human confirmation.

## Related Notes

- [[Proposed plan API-First Web Product Plan - Trade Capture Draft Workflow]]
- [[brainstorm-20260427-api-first-web-product-and-trader-language-input]]
- [[trade-lifecycle-and-objects]]
- [[deterministic-rules-vs-contextual-intelligence]]
- [[context-intelligence-layer]]
- [[product-roadmap-and-learning-boundaries]]
