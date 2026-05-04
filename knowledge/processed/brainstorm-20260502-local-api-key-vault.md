---
title: Local API Key Vault For Trading System
type: brainstorm
status: processed
tags: [trading-system, brainstorm, api-keys, local-key-vault, security]
created: 2026-05-02
---

# Local API Key Vault For Trading System

## Trigger

While planning Milestone 7F, the browser trade-capture workspace raised the question of API-key storage for LiteLLM/cloud providers and market data providers.

## Raw Input

The user referenced a prior local encrypted key-management approach from:

```text
C:\Users\bosto\dockerstuff\py-coding-agent\docs\HOW-TO-SETUP-KEYS.md
C:\Users\bosto\dockerstuff\py-coding-agent\py_mono\session\session_manager.py
```

The referenced pattern uses:

- `cryptography.fernet.Fernet` symmetric encryption
- a master key in an OS environment variable named `LLM_MASTER_KEY`
- encrypted key storage in a `.keys.enc` file
- runtime key-management commands such as `/key groq <api_key>`, `/key list`, and `/key remove`
- key resolution from encrypted storage first, then environment variables, then missing key
- leak prevention by not printing stored secrets

The user wondered whether trading-system needs an API vault at some point.

## Observations

- The current trading-system credential boundary is environment-variable based.
- Massive.com currently uses `MASSIVE_API_KEY`.
- LiteLLM/Ollama local parsing does not need a cloud API key, but future LiteLLM providers such as Groq, OpenAI, or Anthropic would.
- The React workspace should not collect, store, display, or manage raw API keys as part of Milestone 7F.
- A local encrypted key store would be a broader security and operations feature than the capture workspace.

## Ideas

- Consider a trading-system-specific local encrypted key store later.
- Use a trading-system-specific master key name such as `TRADING_SYSTEM_MASTER_KEY`, rather than reusing `LLM_MASTER_KEY`.
- Store encrypted provider keys under a local ignored path such as `.trading-system/keys.enc`.
- Resolve keys in this order:
  - encrypted local key store
  - environment variable fallback
  - clear missing-key error
- Start with CLI management before adding any API or UI management surface.
- Keep provider keys out of frontend responses, logs, snapshots, docs examples, tests, and committed files.

## Questions

- Should key-vault work happen before cloud LiteLLM providers are added?
- Should market-data provider keys and LLM provider keys share one encrypted store?
- Should the encrypted key file live beside the JSON store under `.trading-system/`?
- Should the feature require a new ADR before implementation?
- Should Docker use a mounted encrypted key file plus host-provided master key?

## Concerns

- Putting key management into the browser too early could create secret-handling risk.
- A local encrypted file is not the same as a cloud KMS or OS credential manager.
- Losing the master key may make stored provider keys unrecoverable.
- Storing encrypted keys in the repo workspace still requires `.gitignore` coverage and careful logging boundaries.
- Key-vault implementation should not distract from Milestone 7 parse/edit/save workflow completion.

## Possible Next Outputs

- ADR candidate for local encrypted credential storage.
- Future milestone issue for local key-vault implementation.
- Documentation note for current environment-variable credential boundaries.
- No action until cloud LLM provider or additional credentialed market-data work makes env-var ergonomics painful.
