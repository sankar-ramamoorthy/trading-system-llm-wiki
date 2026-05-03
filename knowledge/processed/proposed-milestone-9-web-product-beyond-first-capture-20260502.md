---
title: Proposed Milestone 9 Web Product Beyond First Capture
type: processed-note
status: processed
tags: [trading-system, milestone-9, web-ui, fastapi, react, market-context, trade-plans]
created: 2026-05-02
---

# Proposed Milestone 9 Web Product Beyond First Capture

This note processes the raw note `Proposed Milestone 9 Plan Web Product Beyond First Capture.md`.

## Status

Milestone 9 is the active next planned slice after completed Milestone 8 Options Chain Ingestion.

The raw note narrows the application repo roadmap's candidate Milestone 9 direction into a plan-first web product implementation plan. It is not recorded as implemented in the application repo yet.

## Purpose

Milestone 9 should make the browser useful after the first trade capture is saved.

The product should move from a single capture screen to a compact local workbench where the trader can:

- list saved trade plans
- inspect plan details
- approve draft plans from the browser
- attach existing market context snapshots to plans

The durable product shape is "plans first." Idea and thesis data should appear through plan detail instead of getting separate management screens in this slice.

## Backend Direction

Add FastAPI plan read endpoints:

```text
GET  /trade-plans?approval_state=&sort=
GET  /trade-plans/{trade_plan_id}
POST /trade-plans/{trade_plan_id}/approve
```

Expected behavior:

- plan list returns summaries with linked idea/thesis context, display symbol/playbook when resolvable, approval state, created time, and linked context count
- plan detail returns idea, thesis, plan fields, rule evaluations, order intents, positions, and linked context metadata
- approve calls `TradePlanningService.approve_trade_plan()` and returns updated plan detail

Add FastAPI context attachment endpoints:

```text
GET  /market-context?instrument_id=&target_type=&target_id=&context_type=&source=
POST /market-context/{snapshot_id}/copy-to-target
```

Expected behavior:

- context list returns metadata summaries only, not full payloads
- copy-to-target accepts a target of `TradePlan` and copies an existing snapshot to the plan through `MarketContextImportService`
- copying creates a new linked snapshot and does not mutate the original snapshot

## Frontend Direction

Extend the React app from a single capture screen into a compact local workbench:

- top-level navigation: Capture, Plans, Context
- Plans is the primary Milestone 9 view
- plan list supports approval-state filtering and newest/oldest sorting
- plan detail shows Idea, Thesis, Plan, linked records, linked market context, and an approve action for draft plans
- context attachment panel lists existing instrument-matching snapshots and attaches by copying the selected snapshot to the plan

The UI should stay local, utilitarian, and workflow-focused. It should deepen the existing web product rather than become a dashboard, analytics product, or execution surface.

## Boundaries

Milestone 9 should not add:

- broker integration
- execution
- order intent creation
- position opening
- fill recording
- generated recommendations
- authentication
- key vault behavior
- Postgres migration
- separate idea or thesis management screens

Browser approval means approval only, matching the existing CLI service boundary. Rule evaluation remains separate and should not become a prerequisite for approval in this milestone.

Market context in the browser should remain metadata-only. Full payload inspection remains outside the Milestone 9 browser scope.

## Validation Direction

Backend API tests should cover:

- list plans with draft/approved filters and sort order
- fetch plan detail with linked idea, thesis, rule evaluations, positions, order intents, and context metadata
- approve a draft plan and persist `approval_state: approved`
- missing plan returns 404 for detail and approval
- list context candidates by instrument and target filters
- copy context to a trade plan without mutating the original snapshot
- mismatched context and plan instruments return a clear error

Frontend verification should cover:

- `npm.cmd run build`
- browser navigation from Capture to Plans to Plan detail
- saved draft plan appears in the list
- approve action updates detail and list state
- context attachment adds a linked context row to plan detail

Regression should include focused API/query/context tests plus the full application test suite.

## Relationship To Current Roadmap

The application repo `DOCS/product-roadmap.md` currently lists Milestone 9 as the next planned web product depth slice with candidate direction for list/detail views, browser plan approval, and browser context attachment.

This processed note makes that candidate direction more concrete, with plan-centered scope and explicit non-goals. The application repo still needs its own milestone issue map or design document before implementation starts.

## Related Pages

- [[milestone-9-web-product-beyond-first-capture]]
- [[api-first-trade-capture-product-vision]]
- [[application-implementation-status]]
- [[product-roadmap-and-learning-boundaries]]
- [[implemented-milestone-8-20260502]]
