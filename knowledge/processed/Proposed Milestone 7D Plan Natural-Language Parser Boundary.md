
  # Milestone 7D Plan: Natural-Language Parser Boundary

  ## Summary

  Implement the parser boundary for trade-capture text without adding API endpoints, UI, save workflow, or
  persistence. 7D should add a swappable parser port, a LiteLLM-backed Ollama adapter, and test fakes so later 7E/7F
  work can call a stable parser interface.

  Default runtime should use LiteLLM Python SDK with host Ollama:
  TRADING_SYSTEM_LLM_MODEL=ollama_chat/llama3.1
  TRADING_SYSTEM_LLM_API_BASE=http://localhost:11434 natively, already mapped to http://host.docker.internal:11434 in
  Docker.

  ## Key Changes

  - Add litellm as a Python dependency and update the lockfile.
  - Add a parser port, e.g. TradeCaptureParser, with parse(source_text: str) -> TradeCaptureDraft.
  - Add TradeCaptureParseError for clear, non-persistent parser failures.
  - Add a LiteLLM parser adapter that:
      - calls litellm.completion
      - uses the configured model and API base
      - requests JSON/structured output
      - validates JSON before mapping into the 7C TradeCaptureDraft
      - calls draft.validation_issues() after mapping so missing fields are surfaced, not guessed
  - Add a fake parser implementation for tests and later API tests.
  - Keep prompt instructions strict: extract only user-authored content; do not suggest trades, invent levels, verify
    claims, approve plans, create order intents, open positions, or record fills.

  ## Behavior

  - Empty or whitespace-only source text fails before provider call.
  - Missing model/API-base configuration fails clearly before provider call.
  - Provider errors, invalid JSON, malformed schema, or non-object output raise TradeCaptureParseError.
  - Unknown or omitted fields become None or empty lists and are reported by the existing draft validation contract.
  - Parser-reported ambiguities map to DraftFieldIssue(issue_type="ambiguous").
  - No parsed result is persisted in 7D.

  ## Documentation

  - Add DOCS/milestone-7d-natural-language-parser-boundary.md.
  - Update DOCS/milestone-7-issue-map.md to mark 7D complete after validation and make 7E next.
  - Update README.md and STATUS.md with parser runtime config, noting Ollama must be running locally.
  - Include no real API keys, prompts with proprietary data, or captured LLM outputs.

  ## Test Plan
  - LiteLLM adapter tests with monkeypatched litellm.completion:
      - ambiguous fields are preserved
      - empty source text fails before provider call
      - missing env/config fails clearly
      - provider exception is wrapped without leaking config
      - invalid JSON and malformed payload fail clearly
  - Regression tests:
      - uv run pytest tests\test_trade_capture_draft.py tests\test_trade_capture_parser.py
      - uv run pytest tests\test_reference_lookup_service.py tests\test_api_health.py
      - uv run pytest

  ## Assumptions

  - 7D follows the current issue map’s LLM-first direction while preserving ADR-008’s parser-port boundary.
  - LiteLLM SDK is used directly, not LiteLLM Proxy.
  - Ollama local is the first acceptance runtime; cloud provider support remains only a future configuration
    possibility.
  - Official LiteLLM docs support the chosen direction: Python SDK completion, Ollama ollama_chat, and JSON/structured
    output support.
    Sources: https://docs.litellm.ai/ , https://docs.litellm.ai/docs/providers/ollama ,
    https://docs.litellm.ai/docs/completion/json_mode
