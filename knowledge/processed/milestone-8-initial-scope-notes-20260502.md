---
title: Milestone 8 Initial Scope Notes (Superseded)
type: processed
status: superseded
tags: [trading-system, milestone-8, planning, scope]
created: 2026-05-02
updated: 2026-05-02
---

# Milestone 8 Initial Scope Notes

Processed from raw note: `MIlestone 8 initial notes before formal scope.md`

**Status: superseded.** The original Milestone 8 direction ("web product beyond first capture") has been reassigned to Milestone 9. Options Chain Ingestion is now accepted as Milestone 8.

---

## Original Direction (Now Milestone 9)

The original candidate direction for Milestone 8 was:

> "Make the web product useful beyond first capture."

Candidate outcomes (all now targeting Milestone 9):

- Web read/detail views — browse saved ideas, theses, and plans in the browser, not just via CLI
- Draft editing — revise a saved plan after capture
- Context attachment from the web UI — link a market snapshot to a plan through the browser
- Plan approval workflow — begin the approve → rule-evaluate flow from the web
- Better daily-use loop — a dashboard or list view of today's active captures

---

## Why It Was Reassigned

After Milestone 7 closed, options data ingestion (yfinance + Massive.com) was identified as a higher-priority next step because:

1. The user has an active Massive.com API key and wants to pull options chains for trade context
2. Options data is purely additive to the existing market context infrastructure — no web work required
3. The web product expansion is still valuable but can follow once market data depth is in place

---

## Accepted Milestone 8 (Supersedes This Note)

**Milestone 8: Options Chain Ingestion**

- yfinance options chain by expiry
- Massive.com options chain by expiry (with greeks where available)
- New CLI command: `fetch-options-chain SYMBOL --expiry DATE --provider yfinance|massive`
- Stored as `context_type: options_chain` MarketContextSnapshot

See `DOCS/product-roadmap.md` for the full near-term sequence.

---

## Related

- [[system-capability-after-milestone-7-20260502]]
- [[implemented-milestone-7h-20260502]]
- [[milestone-7-api-first-trade-capture-issue-map]]
