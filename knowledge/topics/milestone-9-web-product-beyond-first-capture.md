---
title: Milestone 9 Web Product Beyond First Capture
type: topic
status: draft
tags: [trading-system, milestone-9, web-ui, fastapi, react, trade-plans, market-context]
created: 2026-05-02
updated: 2026-05-02
---

# Milestone 9 Web Product Beyond First Capture

Milestone 9 is the next planned slice after completed Milestone 8 Options Chain Ingestion.

The goal is to make the browser useful after the first trade capture is saved. The durable product direction is a plan-centered local workbench: saved plan list, plan detail, browser approval for draft plans, and browser attachment of existing market context snapshots.

This is a planning page, not an implementation closeout.

## Product Shape

Milestone 7 delivered first capture:

```text
raw trader language -> editable Idea/Thesis/Plan draft -> explicit save
```

Milestone 9 should make those saved records usable in the browser:

```text
Capture -> Plans -> Plan detail -> approve draft plan -> attach existing context
```

The milestone should be "plans first." The plan is the daily workflow object that ties the idea, thesis, rule evaluations, order intents, positions, and linked context together.

## Expected User Workflow

1. Capture or save a draft trade plan through the existing browser capture workflow.
2. Open the Plans view.
3. Filter by approval state and sort newest or oldest.
4. Open a plan detail view.
5. Inspect the linked idea, thesis, plan fields, rule evaluations, order intents, positions, and market-context metadata.
6. Approve a draft plan from the browser.
7. Attach an existing instrument-matching context snapshot to the plan by copying it to the plan target.

## Backend Scope

Milestone 9 should add FastAPI read and workflow endpoints over existing service boundaries.

Plan endpoints:

```text
GET  /trade-plans?approval_state=&sort=
GET  /trade-plans/{trade_plan_id}
POST /trade-plans/{trade_plan_id}/approve
```

Context endpoints:

```text
GET  /market-context?instrument_id=&target_type=&target_id=&context_type=&source=
POST /market-context/{snapshot_id}/copy-to-target
```

The plan list should return summaries with approval state, created time, linked context count, and display labels for symbols and playbooks when resolvable.

The plan detail should return the linked idea, thesis, plan fields, rule evaluations, order intents, positions, and linked market-context metadata.

The approve endpoint should call the existing planning service approval behavior and return updated plan detail.

The context copy endpoint should use the existing market context import/copy service behavior. It should create a new linked snapshot and leave the original snapshot unchanged.

## Frontend Scope

Extend the React/Vite app into a compact local workbench.

Expected navigation:

- Capture
- Plans
- Context

Plans is the primary Milestone 9 view.

Expected Plans behavior:

- list saved plans
- filter by approval state
- sort newest or oldest
- open plan detail
- approve draft plans
- show linked market-context metadata
- list instrument-matching existing snapshots for attachment
- attach context by copying the selected snapshot to the current plan

The Context view can remain a supporting discovery surface. Full market-context payload rendering is not required and should stay outside this milestone.

## Explicit Boundaries

Milestone 9 should not add:

- broker integration
- execution
- order intent creation
- position opening
- fill recording
- generated recommendations
- authentication
- API-key vault behavior
- Postgres migration
- separate idea management screens
- separate thesis management screens
- full market-context payload rendering in the browser

Browser approval means approve only. Deterministic rule evaluation remains a separate workflow and should not become a Milestone 9 approval prerequisite.

JSON persistence and existing service boundaries remain the active implementation foundation.

## Acceptance Direction

Backend validation should prove:

- plan list filtering and sorting
- complete plan detail composition
- draft plan approval persistence
- missing plan errors for detail and approval
- metadata-only context candidate listing
- context copy-to-plan behavior without mutating the original snapshot
- clear error for mismatched context and plan instruments

Frontend validation should prove:

- production build passes
- navigation works from Capture to Plans to Plan detail
- saved draft plans appear in the list
- approve updates the detail and list state
- context attachment adds a linked context row to plan detail

Regression should include focused API/query/context tests plus the full application test suite.

## Source And Alignment

This page was promoted from [[proposed-milestone-9-web-product-beyond-first-capture-20260502]] and reconciled against the linked application repo roadmap and status on 2026-05-02.

No contradiction was found with the application repo. The app repo currently treats Milestone 9 as the next planned slice, while this page records the narrower proposed plan-centered shape.

## Related Pages

- [[api-first-trade-capture-product-vision]]
- [[application-implementation-status]]
- [[product-roadmap-and-learning-boundaries]]
- [[milestone-7-api-first-trade-capture-issue-map]]
- [[implemented-milestone-8-20260502]]
