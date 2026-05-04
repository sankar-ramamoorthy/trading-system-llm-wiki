---
title: API-First Web Product And Trader-Language Input Brainstorm
type: brainstorm
status: processed
tags: [trading-system, brainstorm, product-vision, api-first, web-ui, trader-language-input]
created: 2026-04-27
---

# API-First Web Product And Trader-Language Input Brainstorm

## Trigger

The user explored practical friction around creating a `TradeIdea` from normal trader language. The current CLI requires UUIDs for `instrument_id` and `playbook_id`, which exposes a larger product issue: the system currently asks the user to think like the implementation instead of like a trader.

This discussion followed the raw proposed plan:

- [[Proposed plan API-First Web Product Plan - Trade Capture Draft Workflow]]

## Raw Input

The user noted that the current CLI requires UUIDs and does not expose friendly commands such as `--symbol NVDA` or `--playbook pullback-to-trend`.

The user expected language like this to be acceptable:

```text
I may take a long swing trade in NVDA using my pullback-to-trend playbook over a days-to-weeks horizon.
Entry: buy at X
Stop: sell below Y
Target: take profit at Z
NVDA is holding the 20DMA, sector leadership is intact, and volume dried up on the pullback.
```

The user observed several likely product needs:

- some kind of modern frontend, probably backed by a microservice-style frontend/backend boundary
- an LLM or parser that converts natural language draft ideas into structured data the system accepts
- extraction of `TradeIdea`, `TradeThesis`, and `TradePlan` from one normal user draft
- a future version where the system can suggest buy levels, stops, or targets
- a future version where the system can check whether thesis claims are supported by market/context data

The user then clarified the near-term sequence:

- all three directions will eventually be required
- first focus on broader product UI vision with frontend/backend/service boundaries
- then focus on trader-language input and how users naturally describe ideas, plans, evidence, risk, and uncertainty
- early versions should parse user language only, not suggest trades
- future claim verification and trade suggestion should remain later capabilities

## Observations

- The UUID problem is a symptom, not the core issue.
- A viable product needs user-facing language such as symbols, playbook names, trader phrases, editable drafts, and confirmation flows.
- The CLI is useful as a power/debug/admin surface, but it should not remain the primary long-term product surface.
- A web UI is valuable because trade capture benefits from editable sections, missing-field indicators, review before save, and eventually charts/context.
- The cleaner engineering direction is API-first: build a local service boundary before or alongside the web UI.
- The first product workflow should optimize fast capture of a raw trade thought.
- The product should distinguish parsing from suggesting.
- The product should distinguish user-authored thesis claims from future system verification of those claims.

## Ideas

- Adopt an API-first local web product direction:
  - existing domain/services remain core
  - FastAPI exposes local workflow endpoints
  - React/Vite becomes the first web UI direction
  - Typer CLI remains available for power/debug/admin workflows
- Build a focused trade-capture workspace:
  - large raw text input
  - parse action
  - editable `TradeIdea`, `TradeThesis`, and `TradePlan` draft panels
  - missing or ambiguous field indicators
  - explicit save action
- Add lookup support so users work with `NVDA` and `pullback-to-trend`, not UUIDs.
- Add a trader-language parser port:
  - deterministic/stub implementation first
  - real LLM implementation later behind the same interface
  - extraction only, no suggestions
- Keep persistence explicit:
  - parsing creates drafts
  - saving creates domain objects
  - plan approval and execution remain separate workflows
- Future thesis verification could decompose claims such as:
  - `NVDA is holding the 20DMA`
  - `sector leadership is intact`
  - `volume dried up on the pullback`
- Future suggestion capabilities could propose candidate levels, but those should be treated as decision support requiring user acceptance, not automatic intent.

## Questions

- What is the minimum useful instrument/playbook lookup model for the first web capture workflow?
- Should instruments and playbooks get their own management screens immediately, or should seed/config data be enough at first?
- How much structure should the parser return for ambiguous text?
- Should unresolved fields block save, or can partial drafts be saved?
- How should the UI show confidence or ambiguity without implying AI certainty?
- When LLM parsing is introduced, should it run locally, through OpenAI, or through a configurable provider boundary?
- What later milestone should introduce claim verification?
- What later milestone should introduce system-suggested candidate entry/stop/target levels?

## Concerns

- A web UI that still requires UUIDs would only hide, not solve, the product problem.
- A frontend that shells out to CLI commands would create weak service boundaries.
- LLM-only input could create silent misinterpretation, inconsistent records, and audit problems.
- Suggesting trades too early could blur the boundary between journaling/discipline and decision automation.
- Claim verification should not become fake certainty; it should return evidence, confidence, and data timestamps.
- The parser must not approve plans, create order intents, open positions, or mutate execution state.
- The product should not migrate to Postgres, broker integration, cloud deployment, or production auth as part of this first workflow unless separately decided.

## Possible Next Outputs

- ADR: API-first web product and trade-capture draft workflow
- Topic page: user-facing product vision for API-first trade capture
- Application issue: add FastAPI service boundary
- Application issue: add trade-capture draft parser port
- Application issue: add React/Vite trade-capture workspace
- Later ADR candidate: LLM parser provider boundary
- Later ADR candidate: thesis claim verification boundary
- Later ADR candidate: system suggestion and human-confirmation boundary
