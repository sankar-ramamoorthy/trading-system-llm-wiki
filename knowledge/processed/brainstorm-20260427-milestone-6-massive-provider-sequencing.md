---
title: Milestone 6 Massive Provider Sequencing Brainstorm
type: brainstorm
status: processed
tags: [trading-system, brainstorm, milestone-6, market-data, providers, yfinance, massive, polygon]
created: 2026-04-27
---

# Milestone 6 Massive Provider Sequencing Brainstorm

## Trigger

After recording ADR-008 for the future API-first web product direction, the user revisited the current implementation roadmap and confirmed that Milestone 6 should be finished before pivoting to the API-first trade-capture work.

## Raw Input

The user recalled that the project had just finished the first part of Milestone 6: market data provider integration using yfinance.

The user then clarified the desired sequence:

```text
Finish Milestone 6.
Plan for access using API from another data provider, Massive.com formerly Polygon.io.
Then work on ADR-008.
```

## Observations

- ADR-008 is accepted as future product architecture, but it should not prematurely interrupt the current Milestone 6 provider work.
- The first yfinance slice proves a prototype provider path for explicit daily OHLCV snapshot fetches.
- A second provider plan would test whether the provider boundary is real and not shaped only around yfinance.
- Massive.com, formerly Polygon.io, is a more serious market-data provider candidate than yfinance.
- The next provider work should remain read-only and snapshot-based.

## Ideas

- Treat Milestone 6 as a provider-boundary hardening milestone:
  - 6A: yfinance daily OHLCV snapshot fetch, already implemented
  - 6B: provider abstraction and provider-selection cleanup
  - 6C: Massive.com provider plan and possibly implementation
  - 6D: Milestone 6 closeout
- Keep yfinance as prototype-grade fallback.
- Treat Massive.com as the next provider to plan, not necessarily an immediate broad implementation.
- Start Massive.com with the same narrow shape as yfinance:
  - daily OHLCV or aggregate bars
  - explicit user-invoked fetch
  - stored as `MarketContextSnapshot`
  - no provider-driven recommendations
  - no live streaming or execution-grade quote behavior
- Use a credential boundary such as `MASSIVE_API_KEY`.
- Decide later whether to use the official Python client or direct REST.

## Questions

- Should Massive.com become the preferred configured provider while yfinance remains fallback?
- Should the first Massive.com slice be daily bars only, matching yfinance, or should it include intraday/options later?
- Does adding Massive.com require a new ADR, or an update/companion to ADR-007?
- Should provider selection appear in CLI flags, configuration, or both?
- Should provider payloads be normalized into one common daily OHLCV snapshot shape across providers?

## Concerns

- Expanding provider scope too quickly could pull the project toward live feeds, options chains, or execution-grade data before the boundary is ready.
- Provider-specific schemas should not leak into the domain model.
- API keys must not be stored in snapshots, logs, or committed config.
- ADR-008 should wait until Milestone 6 has a cleaner provider boundary or an explicit closeout.

## Possible Next Outputs

- Application roadmap update for Milestone 6 sequencing
- Milestone 6 design update naming Massive.com as the next provider planning target
- ADR candidate: Massive.com provider boundary
- Implementation issue: provider registry/provider selection
- Implementation issue: Massive.com daily OHLCV snapshot adapter
