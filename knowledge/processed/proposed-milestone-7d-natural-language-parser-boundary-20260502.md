---
title: Proposed Milestone 7D Natural-Language Parser Boundary
type: processed-note
status: processed
tags: [trading-system, milestone-7, parser-boundary, litellm, ollama, trade-capture]
created: 2026-05-02
---

# Proposed Milestone 7D Natural-Language Parser Boundary

This note processes the raw proposed plan for Milestone 7D.

## Status

At the time this proposal was captured, Milestone 7D was not implemented in the application repo yet. The application repo state then was:

- 7A Dockerized Runtime Foundation: complete
- 7B Reference Lookup Foundation: complete
- 7C Trade Capture Draft Contract: complete
- 7D Natural-Language Parser Boundary: next proposed slice

Supersession note: a later raw implementation note on 2026-05-02 recorded that Milestone 7D was implemented. See [[implemented-milestone-7d-20260502]] for the current state.

## Proposed Scope

7D should add the parser boundary for trade-capture text without adding API endpoints, frontend UI, save workflow, or persistence.

The intended implementation shape is:

- `litellm` as a Python dependency
- a swappable parser port such as `TradeCaptureParser`
- `parse(source_text: str) -> TradeCaptureDraft`
- `TradeCaptureParseError` for clear parser failures
- LiteLLM-backed parser adapter using host Ollama as the first runtime
- fake parser implementation for deterministic tests and later API tests

The parser output should map into the already implemented 7C `TradeCaptureDraft` contract.

## Runtime Configuration

Proposed native local defaults:

```text
TRADING_SYSTEM_LLM_MODEL=ollama_chat/llama3.1
TRADING_SYSTEM_LLM_API_BASE=http://localhost:11434
```

Docker should continue using the 7A host mapping:

```text
http://host.docker.internal:11434
```

LiteLLM SDK usage should be direct, not LiteLLM Proxy.

## Behavior

The parser must extract only user-authored trade-capture content.

It must not:

- suggest trades
- invent entry, stop, target, or risk levels
- verify thesis claims
- approve plans
- create order intents
- open positions
- record fills
- persist parsed results

Expected failure behavior:

- empty or whitespace-only source text fails before provider call
- missing model or API-base config fails before provider call
- provider exceptions are wrapped without leaking secrets or unnecessary config details
- invalid JSON, malformed schema, or non-object output fails clearly
- unknown or omitted fields map to `None` or empty lists and are surfaced through the 7C validation contract
- parser-reported ambiguity maps to `DraftFieldIssue(issue_type="ambiguous")`

## Proposed Tests

- ambiguous fields are preserved as draft issues
- empty source text fails before provider call
- missing model/API-base config fails clearly
- provider exception is wrapped
- invalid JSON and malformed payload fail clearly
- existing draft, reference lookup, and API health tests still pass
- full suite still passes

## External Documentation Check

Official LiteLLM docs were checked on 2026-05-02. The docs support the broad direction:

- the Python SDK exposes `completion()`
- Ollama can be called through LiteLLM with `api_base`
- LiteLLM recommends `ollama_chat` for better Ollama responses
- JSON mode and structured output are documented

## Related Pages

- [[milestone-7-api-first-trade-capture-issue-map]]
- [[implemented-milestone-7c-20260502]]
- [[api-first-trade-capture-product-vision]]
- [[application-implementation-status]]
