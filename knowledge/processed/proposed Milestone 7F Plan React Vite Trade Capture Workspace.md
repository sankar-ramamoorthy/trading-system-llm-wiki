• Proposed Plan


  # Milestone 7F Plan: React/Vite Trade Capture Workspace

  ## Summary

  Build the first usable browser workspace for trade capture. Replace the current status-only frontend with a focused React screen that lets the user
  paste trader-language text, parse it through the 7E API, edit the resulting Idea/Thesis/Plan draft fields, explicitly save the confirmed draft, and see
  the saved IDs.

  7F is frontend-only plus docs/tests/build validation. It uses the existing backend endpoints and does not add new backend behavior unless a tiny API
  compatibility fix is required.

  ## Key Changes

  - Replace the current runtime shell in frontend/src/App.tsx with a single trade-capture workspace.
  - Keep API health/reference awareness visible but secondary, not the primary experience.
  - Add typed frontend API helpers for:
      - GET /health
      - GET /reference/instruments
      - GET /reference/playbooks
      - POST /trade-capture/parse
      - POST /trade-capture/save
      - GET /trade-capture/saved/{trade_plan_id} only if needed after save
  - Model frontend draft state to match the 7E API shape:
      - Idea: symbol, playbook slug, purpose, direction, horizon
      - Thesis: reasoning, supporting evidence, risks, disconfirming signals
      - Plan: entry criteria, invalidation, targets, risk model, sizing assumptions
  - Render missing/ambiguous field issues next to the affected fields using stable paths like TradeIdea.instrument_symbol.
  - Add explicit UI states:
      - initial empty draft
      - parsing
      - parsed with issues
      - parsed and ready to save
      - saving
      - saved
      - parse/save error
      - API unreachable

  ## UI Behavior

  - First screen should be the actual workspace, not a landing page.
  - Layout:
      - left/top raw text capture area with parse action
      - editable Idea, Thesis, and Plan sections
      - compact reference/status strip showing API status plus available instrument/playbook counts
      - saved-result summary after save
  - Editing behavior:
      - scalar fields use text/select-style controls where practical
      - list fields use newline-separated textarea values for v1
      - user can edit parsed values before saving
      - clearing a required field immediately shows it as not ready to save once validation issues are available
  - Parse behavior:
      - parse button disabled when source text is blank or parse is already running
      - parse errors are shown without clearing the user’s raw text
      - parsed response replaces editable draft state
  - Save behavior:
      - save button disabled while saving
      - save sends the current editable draft, not the original raw text
      - validation errors from the API are shown next to fields and in a compact error area
      - successful save displays idea/thesis/plan IDs and core summary fields
  - The UI must not include approval, rule evaluation, order intent, position, fill, broker, recommendation, or claim-verification actions.

  ## Styling

  - Keep the interface utilitarian and work-focused.
  - Use a dense but readable two-column desktop layout and single-column mobile layout.
  - Avoid marketing-style hero sections, oversized decorative cards, gradients, or ornamental visuals.
  - Use restrained colors with clear state styling for errors, readiness, and saved success.
  - Use stable responsive dimensions so controls do not jump during parse/save/error transitions.
  ## Tests And Validation
    7F.
  - Required validation:
      - npm run build from frontend/
      - uv run pytest tests\test_api_trade_capture.py tests\test_api_health.py
      - uv run pytest
  - Manual acceptance after build:
      - start Docker or local API/web runtime
      - confirm health/reference counts load
      - paste an NVDA pullback setup
      - parse populates editable Idea/Thesis/Plan sections
      - missing fields display clearly if parser omits them
      - user can edit fields
      - save creates linked records and shows generated IDs
      - no approval/execution actions are visible

  ## Documentation

  - Add DOCS/milestone-7f-react-trade-capture-workspace.md.
  - Mark 7F complete in DOCS/milestone-7-issue-map.md after validation.
  - Update README.md Local Web Runtime / Trade Capture API section to describe the browser workspace.
  - Update STATUS.md with 7F completion and set next slice to 7G.
  - Separately, after implementation mode resumes, create a raw knowledge-base brainstorm note for the local API-key vault discussion:
      - knowledge/raw/brainstorm-20260502-local-api-key-vault.md
      - keep it raw, non-canonical, and separate from 7F implementation docs.

  ## Assumptions

  - 7F does not change backend contracts unless frontend integration reveals a small compatibility bug.
  - 7G remains the end-to-end workflow closeout/polish slice for Docker/browser acceptance and persistence verification.
  - API-key vault work is explicitly deferred from 7F and captured as a raw knowledge-base note, not implemented now.
  - The frontend continues using React/Vite with no new UI framework.