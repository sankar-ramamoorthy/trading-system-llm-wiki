• Proposed Plan


  # Milestone 7: API-First Trade Capture Workspace

  ## Summary

  Milestone 7 should deliver the first usable local web product workflow from ADR-008: a React/Vite trade-capture
  screen backed by FastAPI. The user can paste normal trader language, parse it through a configurable LiteLLM parser
  boundary, edit the resulting TradeIdea, TradeThesis, and TradePlan draft fields, then explicitly save linked domain
  records.

  Done means this works end to end locally with seeded instrument/playbook lookup data, local JSON persistence, and no
  CLI shell-out.

  ## Milestone Issues

  1. Milestone 7A: Reference Lookup Foundation
      - Add first-class local lookup support for instruments by symbol and playbooks by slug/name.
      - Use seeded local JSON reference data; unresolved symbols/playbooks block save.
      - Keep UUIDs internal. The user-facing API/UI works with symbols and playbook slugs.
  2. Milestone 7B: Trade Capture Draft Contract
      - Add draft types/schemas for TradeIdeaDraft, TradeThesisDraft, and TradePlanDraft.
      - Include missing/ambiguous field reporting.
      - Required save fields: symbol, playbook, purpose, direction, horizon, thesis reasoning, entry criteria,
        invalidation.
      - Optional save fields: supporting evidence, risks, disconfirming signals, targets, risk model, sizing
        assumptions.
  3. Milestone 7C: LiteLLM Parser Boundary
      - Add a parser port with a LiteLLM implementation and a fake implementation for tests.
      - Default local runtime: Ollama through LiteLLM, configured as TRADING_SYSTEM_LLM_MODEL=ollama_chat/llama3.1 and
        TRADING_SYSTEM_LLM_API_BASE=http://localhost:11434.
      - Parser must extract only user-authored content. It must not suggest trades, invent entry/stop/target levels,
        verify claims, approve plans, or create execution intent.
      - Invalid JSON, provider errors, missing model config, or unavailable Ollama return clear parse errors without
        persistence.
  4. Milestone 7D: FastAPI Service Boundary
      - Add a local FastAPI app over existing services and JSON repositories.
      - Endpoints:
          - health check
          - instrument/playbook lookup
          - parse trade-capture text into editable draft
          - save confirmed draft into linked TradeIdea, TradeThesis, and TradePlan
          - retrieve saved result summary
      - The API creates records only through existing service boundaries.
  5. Milestone 7E: React/Vite Capture Workspace
      - Add a focused local web UI:
          - raw trade-language input
          - parse action
          - editable Idea, Thesis, and Plan sections
          - missing/ambiguous field indicators
          - explicit save action
          - saved result summary with generated IDs
      - The UI must not expose UUIDs as primary input and must not include approval, order intent, position, fill,
        broker, or recommendation actions.
  6. Milestone 7F: Local Dev And Configuration
      - Add app commands/docs for running API and frontend locally.
      - Add .env.example with placeholders only.
      - Keep real .env ignored.
      - Document Ollama/LiteLLM setup and how to override model/provider through env vars.
  7. Milestone 7G: Closeout
      - Update README, STATUS, PROJECT, roadmap, and knowledge-base pages.
      - Record validation and remaining boundaries.
      - Mark Milestone 7 complete only after API, UI, parser boundary, lookup, save workflow, and docs are all
        working.

  ## Done Criteria

  Milestone 7 is done when:

  - A local user can run the API and web UI.
  - The user can paste a trade idea such as an NVDA pullback setup.
  - Saving creates linked domain records in the existing JSON store.
  - No broker, execution, plan approval, claim verification, generated recommendations, Postgres migration, production
    auth, or cloud deployment is introduced.

  ## Test Plan

  - Unit tests for reference lookup, draft validation, parser port behavior, and fake parser output.
  - API tests for lookup, parse, save, missing-field rejection, unresolved symbol/playbook rejection, and saved result
    retrieval.
  - UI tests or documented manual acceptance for parse-edit-save flow.
  - Regression tests proving existing CLI trade workflows still pass.
  - Full suite: uv run pytest.

  ## Assumptions

  - Milestone 7 uses LiteLLM Python SDK directly, not LiteLLM Proxy.
  - Ollama local is the default acceptance runtime; cloud providers remain configurable later through LiteLLM model/
    env settings.
  - Reference lookup is seeded, not auto-created during save.
  - Parser output is extraction-only and treated as draft data until explicit user save.
  - Sources checked: https://docs.litellm.ai/ and https://docs.litellm.com.cn/docs/providers/ollama.