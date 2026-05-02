---
title: Proposed Milestone 7F React Trade Capture Workspace
type: processed-note
status: processed
tags: [trading-system, milestone-7, react, vite, trade-capture, web-ui]
created: 2026-05-02
---

# Proposed Milestone 7F React Trade Capture Workspace

This note processes the raw note `What 7F Is Supposed To Do.md`.

## Status

Milestone 7F is the next planned slice after implemented Milestone 7E.

7F is not recorded as implemented in the application repo yet.

## Purpose

7F should replace the current frontend runtime/status shell with the first real web trade-capture workspace.

The existing frontend only checks API health and shows seeded instrument/playbook counts. 7F should make the trade-capture workflow usable in the browser on top of the 7E backend.

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

## Related Pages

- [[milestone-7-api-first-trade-capture-issue-map]]
- [[implemented-milestone-7e-20260502]]
- [[api-first-trade-capture-product-vision]]
- [[application-implementation-status]]
