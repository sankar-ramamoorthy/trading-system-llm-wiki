
  ---
  title: Milestone 7 And Milestone 8 Planning Restart
  type: raw-note
  status: raw
  tags: [trading-system, milestone-7, milestone-8, api-first, trade-capture, Docker]
  created: 2026-04-28 10:50 PM EDT
  ---

  # Milestone 7 And Milestone 8 Planning Restart

  Next session should resume roadmap planning after Milestone 6 closeout.

  ## Immediate Context

  A proposed Milestone 7 plan was created and saved in `knowledge/raw/`.

  Before accepting or implementing that plan, re-read and reconcile it against:

  - `knowledge/raw/brainstorm-20260427-api-first-web-product-and-trader-language-input.md`
  - `knowledge/topics/api-first-trade-capture-product-vision.md`
  - `C:\Users\bosto\dockerstuff\trading-system\DOCS\ADR\008-api-first-web-product-and-trade-capture-drafts.md`

  ## Current Working Thought

  Milestone 7 likely should be:

  > Capture and save user-authored trade drafts through a local API-first web workflow.

  It should probably include:

  - FastAPI backend
  - React/Vite capture workspace
  - configurable LiteLLM parser boundary
  - local Ollama as default runtime
  - seeded instrument/playbook lookup
  - explicit parse/edit/save flow
  - no trade suggestions
  - no claim verification
  - no approval/execution/fill workflow

  ## Open Question

  The user wants to think more about the Milestone 7 plan before accepting it.

  Also think about using docker with microservices for backend and front end. in the past i have used ghcr.io/astral-sh/uv:python3.12-bookworm-slim as a starting point. 

  Also discuss Milestone 8 at the outcome level, not issue-by-issue yet.

  Possible Milestone 8 direction:

  > Make the web product useful beyond first capture.

  Candidate outcomes may include:

  - web retrieval/detail views for saved ideas/theses/plans
  - editing or revising draft plans
  - context attachment from the web UI
  - review or planning workflow expansion
  - better daily-use loop around captured trades

  Do not lock Milestone 8 issues until Milestone 7 scope is stable.

  ## Next Session Order

  1. Re-read the saved Milestone 7 raw plan.
  2. Re-read the API-first brainstorm raw note.
  3. Compare the plan against ADR-008 and the product vision topic.
  4. Decide whether Milestone 7 should stay as proposed or be narrowed.
  5. Sketch what Milestone 8 should accomplish at the milestone-outcome level.
  6. docker for backend and frontend