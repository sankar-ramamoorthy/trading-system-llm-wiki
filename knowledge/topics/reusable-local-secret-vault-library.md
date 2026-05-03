---
title: Reusable Local Secret Vault Library
type: topic
status: partially-accepted
tags: [trading-system, security, api-keys, key-vault, reusable-library, adr-candidate]
created: 2026-05-02
updated: 2026-05-03
---

# Reusable Local Secret Vault Library

This page captures a possible reusable library direction for local API-key and secret management.

This direction is now partially accepted in the trading-system application repo. Milestone 10 implemented a small app-local `local_secret_vault` library and ADR-010 accepted the local secret vault boundary. The broader idea of extracting the vault as a reusable cross-project package remains future work.

## Current Stance

A reusable local secret vault is realistic if the reusable scope stays small.

The reusable core should be a library, not a full product. It should provide encrypted local secret storage and secret resolution. Each application should own its own commands, docs, provider names, runtime integration, and UI/API exposure decisions.

## Library-First Shape

The library should provide a small provider-agnostic API:

- store a secret by logical name
- retrieve a secret by logical name
- delete or rotate a secret
- list secret names without values
- resolve a secret using encrypted store first and environment fallback second
- return missing-secret errors without leaking secret values
- provide redacted display helpers

The reusable package could be organized around:

```text
local_secret_vault/
  crypto.py
  store.py
  resolver.py
  models.py
  errors.py
```

## Project Adaptation

Each project should adapt the library through project-specific configuration.

For trading-system, a possible future shape is:

```text
TRADING_SYSTEM_MASTER_KEY
.trading-system/keys.enc
MASSIVE_API_KEY
OPENAI_API_KEY
GROQ_API_KEY
```

The trading-system integration should preserve the current environment-variable behavior until an ADR accepts a new secret-resolution boundary.

## Non-Scope For The Reusable Core

The first reusable library should not try to own:

- browser-based secret entry
- project-specific CLI command names
- API management endpoints
- provider-specific policy
- Docker secret orchestration
- cloud secret managers
- team sharing
- production auth
- audit logging
- full OS-keychain support across platforms

Those can be separate adapters or later product decisions.

## Trading-System Guardrails

- Do not put key management into Milestone 7F.
- Do not let the frontend collect, display, store, or transmit raw API keys as part of the trade-capture workspace.
- Do not commit encrypted key files.
- Do not store secrets in snapshots, logs, docs examples, tests, or API responses.
- Treat master-key management as the real security boundary.

## Accepted Application Shape

Milestone 10 Secure Credentials accepted and implemented the first app-local version:

- Fernet-encrypted local vault payload
- OS keychain-backed master-key storage
- `.trading-system/keys.enc` vault file
- CLI-only secret management
- vault-first, environment-fallback resolution
- Massive.com provider credential resolution through the vault boundary

See [[milestone-10-secure-credentials]].

## ADR Candidate

A future extraction ADR or design note should decide:

- whether the app-local `local_secret_vault` should be extracted for reuse across projects
- whether the library should be created outside the application repo or prototyped locally first
- encryption approach
- master-key naming and storage expectations
- vault file path
- secret lookup precedence
- CLI/API management boundary
- Docker and local development behavior
- redaction and testing requirements

## Milestone Assignment

Accepted and completed as **Milestone 10: Secure Credentials**:

- ADR-010 records the key vault boundary.
- A small app-local vault library was implemented.
- Vault-first, environment-fallback secret resolution was wired into Massive.com provider configuration.

Key vault work should land before Alpaca/paper trading (Milestone 11) so broker credentials (higher stakes) use the vault from day one.

## Related Pages

- [[api-key-vault-discussion-20260502]]
- [[key-vault-encryption-brainstorm-20260502]]
- [[data-and-platform-strategy]]
- [[milestone-6-market-data-provider-boundary]]
- [[milestone-7-api-first-trade-capture-issue-map]]
