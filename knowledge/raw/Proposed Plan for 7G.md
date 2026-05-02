╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌
 Milestone 7G: End-to-End Save Workflow

 Context

 Milestones 7A–7F are complete. The backend parse/save service (7E) and React/Vite UI workspace (7F) are both in place and tested (216 tests pass). 7G is
 the stricter acceptance slice that validates the full chain works together in a real Docker/browser environment — not just unit and integration tests. No
 new features are added; the goal is to confirm that the already-built parse → edit → save → persist workflow is solid end-to-end.

 Issue map definition (from knowledge/topics/milestone-7-api-first-trade-capture-issue-map.md, line 288):

 ▎ Wire parse, edit, and save all the way through local JSON persistence. Saving should create linked TradeIdea, TradeThesis, and TradePlan records only
 ▎ after explicit user confirmation.

 ---
 Steps

 1. Docker stack acceptance run

 - Start the full stack (docker compose up) in the application repo
 - Confirm all containers come up healthy (API health strip should show green)

 2. Browser manual walkthrough — golden path

 In the running browser UI, exercise the full workflow:
 1. Enter raw trader text in the input
 2. Click Parse → confirm draft sections populate with TradeIdea, TradeThesis, TradePlan fields
 3. Edit one or more draft fields
 4. Click Save → confirm the saved-result summary appears with generated IDs

 3. Persistence verification

 - After saving, inspect the local JSON store (the *.json data files in the application repo)
 - Confirm linked TradeIdea, TradeThesis, and TradePlan records were written and are retrievable
 - Optionally: call GET /trade-capture/saved/{trade_plan_id} directly to verify API round-trip

 4. Edge-case / error state validation

 Walk through the known UI states defined in the 7F spec:
 - Empty draft (initial state)
 - Parsing in progress
 - Parsed with issues (missing/ambiguous fields shown)
 - Parsed and ready to save
 - Save in progress
 - Saved (result summary)
 - Parse or save error
 - API unreachable

 5. Final workflow polish (if needed)

 - Fix any rough edges surfaced during manual testing
 - No scope expansion: do NOT add plan approval, rule evaluation, order intents, positions, fills, broker actions, recommendations, or auth

 6. Record validation

 Update application repo STATUS.md and this knowledge base with:
 - Commands run and pass/fail results
 - Any issues found and resolved
 - Confirmation that 7G acceptance criteria are met

 ---
 Critical Files

 ┌─────────────────────────────────────────────────────────────────────────────┬─────────────────────────────────────────┐
 │                                    File                                     │                  Role                   │
 ├─────────────────────────────────────────────────────────────────────────────┼─────────────────────────────────────────┤
 │ C:\Users\bosto\dockerstuff\trading-system\STATUS.md                         │ Next-slice pointer (currently 7G)       │
 ├─────────────────────────────────────────────────────────────────────────────┼─────────────────────────────────────────┤
 │ C:\Users\bosto\dockerstuff\trading-system\docker-compose.yml                │ Stack entry point                       │
 ├─────────────────────────────────────────────────────────────────────────────┼─────────────────────────────────────────┤
 │ C:\Users\bosto\dockerstuff\trading-system\frontend\                         │ React/Vite workspace (7F output)        │

 6. Record validation

 Update application repo STATUS.md and this knowledge base with:
 - Commands run and pass/fail results
 - Any issues found and resolved
 - Confirmation that 7G acceptance criteria are met

 ---
 Critical Files

 ┌─────────────────────────────────────────────────────────────────────────────┬─────────────────────────────────────────┐
 │                                    File                                     │                  Role                   │
 ├─────────────────────────────────────────────────────────────────────────────┼─────────────────────────────────────────┤
 │ C:\Users\bosto\dockerstuff\trading-system\STATUS.md                         │ Next-slice pointer (currently 7G)       │
 ├─────────────────────────────────────────────────────────────────────────────┼─────────────────────────────────────────┤
 │ C:\Users\bosto\dockerstuff\trading-system\docker-compose.yml                │ Stack entry point                       │
 ├─────────────────────────────────────────────────────────────────────────────┼─────────────────────────────────────────┤
 │ C:\Users\bosto\dockerstuff\trading-system\frontend\                         │ React/Vite workspace (7F output)        │
 ├─────────────────────────────────────────────────────────────────────────────┼─────────────────────────────────────────┤
 │ C:\Users\bosto\dockerstuff\trading-system\services\trade_capture_service.py │ Backend parse/save logic (7E output)    │
 ├─────────────────────────────────────────────────────────────────────────────┼─────────────────────────────────────────┤
 │ knowledge/topics/milestone-7-api-first-trade-capture-issue-map.md           │ Issue map with 7G definition (line 286) │
 └─────────────────────────────────────────────────────────────────────────────┴─────────────────────────────────────────┘

 ---
 Scope Boundaries

 7G does not add:
 - Plan approval
 - Rule evaluation
 - Order intent creation
 - Position opening / fill recording
 - Broker integration
 - Recommendations or claim verification
 - API key vault behavior
 - Production auth, cloud deployment, or Postgres migration

 ---
 Verification

 Done when:
 - docker compose up starts cleanly
 - Browser golden-path walkthrough completes without errors
 - Local JSON store contains the saved TradeIdea, TradeThesis, and TradePlan records
 - All known UI error states render correctly
 - Full test suite still passes (uv run pytest: 216+ passed)
 - STATUS.md in application repo updated to reflect 7G complete / 7H next
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌
