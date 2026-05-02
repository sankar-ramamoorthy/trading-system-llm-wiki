---
title: Proposed Milestone 7F React Trade Capture Workspace
type: processed-note
status: processed
tags: [trading-system, milestone-7, react, vite, trade-capture, web-ui]
created: 2026-05-02
---

# Proposed Milestone 7F React Trade Capture Workspace

This note processes the raw notes `What 7F Is Supposed To Do.md` and `proposed Milestone 7F Plan React Vite Trade Capture Workspace.md`.

## Status

Milestone 7F is the next planned slice after implemented Milestone 7E.

7F is not recorded as implemented in the application repo yet.

Supersession note: a later raw implementation note on 2026-05-02 recorded that Milestone 7F was implemented. See [[implemented-milestone-7f-20260502]] for the current state.

## Purpose

7F should replace the current frontend runtime/status shell with the first real web trade-capture workspace.

The existing frontend only checks API health and shows seeded instrument/playbook counts. 7F should make the trade-capture workflow usable in the browser on top of the 7E backend.

7F should be frontend-only plus documentation, tests, and build validation. It should use the existing 7E backend endpoints and avoid backend changes unless a small API compatibility fix is required.

## User Flow

1. Paste normal trader language into a large input.
2. Click parse.
3. The UI calls `POST /trade-capture/parse`.
4. The UI renders three editable sections:
   - Idea: symbol, playbook, purpose, direction, horizon
   - Thesis: reasoning, evidence, risks, disconfirming signals
   - Plan: entry criteria, invalidation, targets, risk model, sizing assumptions
5. Missing or ambiguous fields are shown clearly next to the relevant section or field.
6. The user edits the draft directly in the browser.
7. The user explicitly clicks save.
8. The UI calls `POST /trade-capture/save`.
9. The UI shows a saved-result summary with generated idea, thesis, and plan IDs.

## Implementation Direction

- Replace the current runtime shell in `frontend/src/App.tsx` with one focused trade-capture workspace.
- Keep API health and reference awareness visible but secondary.
- Add typed frontend API helpers for health, reference lookup, parse, save, and saved-result retrieval if needed after save.
- Model frontend draft state to match the 7E API shape.
- Render missing and ambiguous field issues next to affected fields using stable paths such as `TradeIdea.instrument_symbol`.

## UI States

7F should explicitly handle:

- initial empty draft
- parsing
- parsed with issues
- parsed and ready to save
- saving
- saved
- parse error
- save error
- API unreachable

## UI Behavior

- First screen should be the workspace, not a landing page.
- Raw text capture should be prominent.
- Editable Idea, Thesis, and Plan sections should be directly visible.
- Reference/API status should be compact and secondary.
- Scalar fields should use text or select-style controls where practical.
- List fields can use newline-separated textareas for v1.
- Parse is disabled when source text is blank or already parsing.
- Parse errors do not clear the user's raw text.
- Parsed responses replace editable draft state.
- Save sends the current editable draft, not the original raw text.
- Save validation errors are shown next to fields and in a compact error area.

## Styling Direction

- Utilitarian and work-focused.
- Dense but readable two-column desktop layout.
- Single-column mobile layout.
- Avoid landing-page hero sections, oversized decorative cards, gradients, or ornamental visuals.
- Use restrained colors with clear state styling for errors, readiness, and saved success.
- Keep control dimensions stable during parse/save/error transitions.

## Boundary

7F must not add:

- plan approval
- rule evaluation
- order intent creation
- position opening
- fill recording
- broker actions
- trade recommendations
- claim verification

7F is primarily the browser usability slice. 7G remains the stricter end-to-end acceptance slice for Docker/browser/manual validation, persistence verification, and final workflow polish.

API-key vault work is explicitly deferred from 7F.

## Validation Direction

Required validation after implementation:

```text
npm run build
uv run pytest tests\test_api_trade_capture.py tests\test_api_health.py
uv run pytest
```

Manual acceptance:

- health/reference counts load
- NVDA pullback setup can be pasted and parsed
- parse populates editable Idea/Thesis/Plan sections
- missing fields display clearly if parser omits them
- user can edit fields
- save creates linked records and shows generated IDs
- no approval/execution actions are visible

## Related Pages

- [[milestone-7-api-first-trade-capture-issue-map]]
- [[implemented-milestone-7e-20260502]]
- [[api-first-trade-capture-product-vision]]
- [[application-implementation-status]]
