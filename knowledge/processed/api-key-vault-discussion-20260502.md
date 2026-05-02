---
title: API Key Vault Discussion
type: processed-note
status: discussion
tags: [trading-system, security, api-keys, key-vault, configuration, local-first]
created: 2026-05-02
---

# API Key Vault Discussion

This note processes `API Key Vault Discussion notes.md` as design discussion input, not as an accepted implementation direction.

## Current Decision

Do not include API-key or key-vault management in Milestone 7F.

The frontend should not collect, display, store, or transmit raw API keys as part of the trade-capture workspace.

Current application behavior remains environment-variable based:

- `TRADING_SYSTEM_LLM_MODEL`
- `TRADING_SYSTEM_LLM_API_BASE`
- future LiteLLM provider keys such as `GROQ_API_KEY` or `OPENAI_API_KEY`
- `MASSIVE_API_KEY` for Massive.com market data

## Discussion Direction

A local encrypted key vault may be useful later, but it should be treated as a separate milestone or ADR candidate because it affects:

- security model
- runtime configuration
- Docker volume behavior
- CLI or API management commands
- log redaction
- documentation
- testing

Possible trading-system direction:

- encrypted local file such as `.trading-system/keys.enc`
- master key only in OS environment, likely `TRADING_SYSTEM_MASTER_KEY`
- provider-key lookup order:
  1. encrypted local key store
  2. environment variable fallback
  3. missing-key error
- never commit encrypted key files
- never expose secrets in API responses
- never pass secrets to the frontend
- management surface should probably be CLI first, with API/UI management only if later justified

## External Reference Pattern

The user mentioned a `py-coding-agent` pattern using Fernet encrypted `.keys.enc`, `LLM_MASTER_KEY`, runtime `/key` commands, and environment fallback. That pattern is relevant as inspiration, but it should not be copied into the trading system without a separate design pass.

## Open Question

When should local encrypted key storage be considered?

Candidate triggers:

- before adding cloud LLM providers through LiteLLM
- before adding more credentialed market-data providers
- only after environment-variable ergonomics become painful

## Related Pages

- [[milestone-7-api-first-trade-capture-issue-map]]
- [[data-and-platform-strategy]]
- [[milestone-6-market-data-provider-boundary]]
- [[api-first-trade-capture-product-vision]]
