---
title: Milestone 7 API-First Trade Capture Issue Map
type: topic
status: active
tags: [trading-system, milestone-7, api-first, web-ui, docker, trade-capture, llm-parser]
created: 2026-04-29
---

# Milestone 7 API-First Trade Capture Issue Map

Milestone 7 should deliver the first local web product slice for API-first trade capture.

The milestone is not one issue. It should be implemented as a sequence of small issues so each slice has a clear acceptance boundary.

## Issue Map

### 7A: Dockerized Runtime Foundation

Create the local runtime shell for the web product.

This includes:

- Docker Compose for backend and frontend runtime surfaces
- FastAPI health endpoint
- Vite React TypeScript frontend shell
- frontend-to-backend health check
- host Ollama configuration placeholders for later parser work

Status: complete in the application repo on 2026-04-29.

Ollama itself should run on the host for now, not as a Compose service.

7A does not implement trade capture, parser behavior, lookup, draft schemas, or save workflow.

### 7B: Reference Lookup Foundation

Add user-facing lookup for instruments and playbooks.

The product should use symbols and playbook slugs, not user-entered UUIDs.

Seeded local reference data is enough for the first version. Management screens are not required yet.

### 7C: Trade Capture Draft Contract

Define editable draft contracts for:

- `TradeIdea`
- `TradeThesis`
- `TradePlan`

The contracts should represent parsed user-authored content, required save fields, optional fields, and missing or ambiguous field reporting.

### 7D: Natural-Language Parser Boundary

Add the LLM-first parser boundary through LiteLLM.

The first target runtime is host Ollama. Later provider routing through LiteLLM can support Groq, OpenAI, or other providers without changing the trade-capture workflow.

The parser must extract only user-authored content. It must not suggest trades, invent levels, verify claims, approve plans, create order intent, open positions, or record fills.

### 7E: FastAPI Trade Capture Service

Expose the trade-capture workflow through FastAPI endpoints.

The API should call existing application services and repositories rather than shelling out to the CLI.

Expected endpoint groups:

- health
- reference lookup
- parse trade-capture text
- save confirmed draft
- retrieve saved result summary

### 7F: React/Vite Trade Capture Workspace

Build the focused web workspace.

The UI should include:

- raw trader-language input
- parse action
- editable Idea, Thesis, and Plan sections
- missing or ambiguous field indicators
- explicit save action
- saved result summary

The screen must not expose approval, order intent, position, fill, broker, or recommendation actions.

### 7G: End-to-End Save Workflow

Wire parse, edit, and save all the way through local JSON persistence.

Saving should create linked `TradeIdea`, `TradeThesis`, and `TradePlan` records only after explicit user confirmation.

### 7H: Milestone Closeout

Close the milestone with tests, documentation, roadmap/status updates, and knowledge-base updates.

Milestone 7 is complete only when the Dockerized web/API workflow can capture normal trader language, parse it into editable drafts, and save linked records without crossing into approval, execution, broker, or recommendation behavior.

## Related Pages

- [[api-first-trade-capture-product-vision]]
- [[product-roadmap-and-learning-boundaries]]
- [[trade-lifecycle-and-objects]]
