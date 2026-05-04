---
title: Milestone 16 Finqual Fundamentals Provider
type: topic
status: complete
tags: [trading-system, milestone-16, finqual, fundamentals, market-context]
created: 2026-05-04
updated: 2026-05-04
---

# Milestone 16 Finqual Fundamentals Provider

Milestone 16 is complete in the linked application repo.

The milestone adds Finqual as a read-only fundamentals and ownership provider behind the existing market-context boundary:

- `fetch-financial-statement --provider finqual`
- `fetch-insider-transactions --provider finqual`
- `fetch-13f --provider finqual`

Finqual output is stored only as `MarketContextSnapshot` records. Statements use `context_type: financial_statement`; insider activity uses `context_type: insider_transactions`; and 13F holdings use `context_type: institutional_holdings_13f`.

The provider uses vault-first, environment-fallback resolution for `FINQUAL_API_KEY`. API keys must not appear in committed docs, tests, snapshot payloads, source refs, or error messages.

## Boundary

Finqual remains advisory external context. It does not define canonical trade meaning, score trades, recommend actions, mutate trade records, schedule refreshes, stream updates, or fall back automatically to another provider.

CIK-based 13F snapshots require an explicit local `instrument_id` or linked target because the app does not yet have a CIK reference registry.

## Validation

Application repo validation recorded on 2026-05-04:

- focused Finqual/provider/CLI tests: 18 passed
- full suite: 323 passed

## Primary Sources

- [[implemented-milestone-16-finqual-fundamentals-provider-20260504]]
- `C:\Users\bosto\dockerstuff\trading-system\DOCS\milestone-16-issue-map.md`
- `C:\Users\bosto\dockerstuff\trading-system\STATUS.md`
- `C:\Users\bosto\dockerstuff\trading-system\README.md`
