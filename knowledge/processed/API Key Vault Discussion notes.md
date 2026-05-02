API Key / Vault Discussion

  For 7F specifically, I would not put key management into the browser. The frontend should never collect, display, store, or transmit raw API keys except
  possibly to a future explicit key-management API, and even that needs careful design.

  Current app behavior is still environment-variable based:

  - TRADING_SYSTEM_LLM_MODEL
  - TRADING_SYSTEM_LLM_API_BASE
  - provider keys if LiteLLM routes to cloud providers later, e.g. GROQ_API_KEY, OPENAI_API_KEY, etc.
  - MASSIVE_API_KEY for Massive.com market data

  Your py-coding-agent pattern is relevant, but I’d treat it as a separate milestone/ADR candidate, not part of 7F. The reason: encrypted local key storage
  affects security model, docs, runtime configuration, Docker volume behavior, CLI/API management commands, and log redaction. That’s bigger than a UI
  capture screen.

  A good trading-system version would probably be:

  - local encrypted file, e.g. .trading-system/keys.enc
  - master key only in OS env, probably a trading-system-specific name like TRADING_SYSTEM_MASTER_KEY
  - key lookup order:
      1. encrypted local key store
      2. environment variable fallback
      3. missing-key error
  - never commit encrypted key file
  - never expose secrets in API responses
  - never pass secrets to frontend
  - management surface could be CLI first, API/UI later only if needed

  Raw Note Draft

  Suggested file later:
  knowledge/raw/brainstorm-20260502-local-api-key-vault.md

  Suggested note title:
  Local API Key Vault For Trading System

  Core note content should capture:

  - Trigger: planning Milestone 7F raised the question of local API-key storage for LiteLLM/cloud providers and market data providers.
  - Existing reference: py-coding-agent uses Fernet encrypted .keys.enc with LLM_MASTER_KEY, runtime /key commands, and env fallback.
  - Idea: trading-system may need a local encrypted key vault, but not inside 7F.
  - Concern: browser UI must not become a place where secrets are stored or leaked.
  - Possible direction: separate ADR or later milestone for TRADING_SYSTEM_MASTER_KEY, encrypted .trading-system/keys.enc, provider-key resolution,
    redaction, tests, and docs.
  - Open question: should this come before cloud LLM providers, before more market data providers, or only when env-var ergonomics become painful.
