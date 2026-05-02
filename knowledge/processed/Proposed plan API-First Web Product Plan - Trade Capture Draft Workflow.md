• Proposed Plan


  # API-First Web Product Plan: Trade Capture Draft Workflow

  ## Summary

  Build the next product stage as an API-first local web product. The CLI remains available for power/debug use, but the intended primary workflow becomes
  a React/Vite web UI backed by a local FastAPI service. The first workflow focuses on fast trade capture: the user enters normal trader language, the
  system parses it into editable draft TradeIdea, TradeThesis, and TradePlan sections, and nothing is persisted until the user explicitly saves.

  This stage should parse user intent, not suggest trades, verify claims, or recommend buy/stop/target levels.

  ## Key Changes

  - Add a FastAPI app boundary over the existing domain/services without replacing the current Typer CLI.
  - Keep the current local JSON store as the active persistence backend.
  - Add user-friendly lookup support so the UI/API can work with symbols and playbook slugs/names instead of requiring UUIDs in the user-facing flow.
  - Add a trader-language parser port with a deterministic/stub implementation first.
  - Parse raw text into a draft object containing:
      - TradeIdeaDraft: symbol/instrument reference, playbook reference, purpose, direction, horizon
      - TradeThesisDraft: reasoning, supporting evidence, risks, disconfirming signals
      - TradePlanDraft: entry criteria, invalidation, targets, risk model, sizing assumptions
  - Draft parsing must return missing/ambiguous fields clearly instead of guessing silently.
  - Saving is a separate explicit action that creates persisted TradeIdea, TradeThesis, and TradePlan records through existing application services.

  ## API And UI Behavior

  - API endpoints should support:
      - listing/searching instruments by symbol/name
      - listing/searching playbooks by slug/name
      - parsing raw trade capture text into editable draft objects
      - saving a confirmed draft into domain objects
      - retrieving created idea/thesis/plan detail after save
  - The first React/Vite screen should be a focused capture workspace:
      - large raw text input
      - parse button
      - three editable sections: Idea, Thesis, Plan
      - missing-field indicators
      - explicit save button
      - saved-result summary with generated IDs/links
  - The UI must not expose UUIDs as primary user input.
  - The UI must not approve plans, create order intents, open positions, or record fills in this first workflow.

  - No trade suggestions.
  - No broker integration.
  - No automatic plan approval or execution.
  - No Postgres migration.
  - No production auth/cloud deployment.
  - No replacement of the existing CLI.

  ## Test Plan

  - API tests:
      - parse normal text into TradeIdea, TradeThesis, and TradePlan draft fields
      - preserve missing fields as explicit missing/ambiguous values
      - resolve symbol/playbook names through lookup data
      - reject save when required fields are unresolved
      - save confirmed drafts through existing services and persist to JSON
  - Service tests:
      - parser port can be swapped without changing API save behavior
      - stub parser is deterministic and auditable
  - UI tests/manual acceptance:
      - user can paste the NVDA example and see editable Idea/Thesis/Plan drafts
      - user can correct parsed fields before saving
      - save creates linked domain records
      - no approval/execution action is available from the first capture screen

  ## Assumptions

  - Primary direction is an API-first web product: FastAPI backend plus React/Vite frontend.
  - CLI remains supported but is no longer treated as the long-term primary product UX.
  - Local JSON persistence remains the active backend for this stage.
  - The first parser uses a port plus stub/deterministic implementation; real LLM integration comes later behind the same interface.
  - Early parser behavior is extraction-only. Future claim verification and system suggestions are later context-intelligence capabilities, not part of
    this first web capture workflow.