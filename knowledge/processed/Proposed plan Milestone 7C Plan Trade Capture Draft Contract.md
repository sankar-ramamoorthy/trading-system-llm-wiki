• Proposed Plan


  # Milestone 7C Plan: Trade Capture Draft Contract

  ## Summary

  Finish Milestone 7C as a narrow contract slice. The goal is to define the editable draft shape used later by the
  parser, FastAPI save workflow, and React capture screen. This step should not parse natural language, call LiteLLM,
  save records, or change the UI beyond what is needed to expose/validate the contract in tests.

  There is already an untracked starter file at src/trading_system/services/trade_capture_draft.py; use it as the
  starting point, add tests, and adjust only where needed.

  ## Key Changes

  - Add stable draft models for:
      - TradeIdeaDraft: instrument_symbol, playbook_slug, purpose, direction, horizon
      - TradeThesisDraft: reasoning, supporting_evidence, risks, disconfirming_signals
      - TradePlanDraft: entry_criteria, invalidation, targets, risk_model, sizing_assumptions
  - Add a top-level TradeCaptureDraft containing the three sections, optional source_text, and explicit missing/
    ambiguous field issues.
  - Define required save fields as:
      - instrument_symbol
      - playbook_slug
      - purpose
      - direction
      - horizon
      - reasoning
      - entry_criteria
      - invalidation
  - Keep optional fields editable but non-blocking.
  - Use stable field paths like TradeIdea.instrument_symbol for API/UI error mapping.
  - Keep unresolved reference lookup out of this slice except for field naming; symbol/playbook resolution remains a
    later save/API concern.

  ## Boundaries

  - No LiteLLM or Ollama integration.
  - No parser implementation.
  - No FastAPI trade-capture endpoint yet.
  - No persistence or save workflow.
  - No frontend capture workspace.
  - No approval, order intent, position, fill, broker, recommendation, or claim-verification behavior.

  ## Test Plan

  - Add focused unit tests for the draft contract:
      - optional fields do not block readiness
      - issue paths are stable and match the section/field names
  - Run:
      - uv run pytest tests\test_trade_capture_draft.py
      - uv run pytest tests\test_reference_lookup_service.py tests\test_api_health.py
      - uv run pytest

  ## Documentation

  - Add DOCS/milestone-7c-trade-capture-draft-contract.md marking 7C complete after validation.
  - Update DOCS/milestone-7-issue-map.md so 7C is complete and 7D is next.
  - Update README.md only if a short note is useful under the Local Web Runtime section; do not document endpoints
    that do not exist yet.
  - Update the knowledge base after implementation in a separate closeout pass if desired.

  ## Assumptions

  - The next step is 7C because 7A and 7B are already complete in the app repo.
  - The existing untracked trade_capture_draft.py is intended work and should be preserved, completed, and tested.
  - Draft contracts live in the service/application layer rather than domain entities because they represent editable,
    unpersisted capture state.
