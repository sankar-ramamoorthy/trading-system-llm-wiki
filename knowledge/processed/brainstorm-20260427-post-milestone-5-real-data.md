---
title: Post-Milestone 5 Real Data Direction
type: brainstorm
status: processed
tags: [trading-system, brainstorm, milestone-5, market-data, roadmap]
created: 2026-04-27
---

# Post-Milestone 5 Real Data Direction

## Trigger

The user asked how many issues remain in Milestone 5 and what comes after Milestone 5, specifically when the project needs to get real market data.

## Raw Input

User:

> thanks. for my knowlegde, how many more issues remain in milestone 5.

Assistant summary:

> There is no fixed issue count recorded for Milestone 5.
>
> What is complete:
>
> 1. Review tags/filtering
> 2. Review quality scores
> 3. Markdown journal export
>
> What still remains from the accepted Milestone 5 scope is mainly:
>
> 1. Local backup / restore / operational support for the local JSON workflow
> 2. Possibly a narrow journal-grade reporting slice, such as simple counts or grouped summaries of completed reviewed trades
>
> So my practical read is: at least 1 more issue remains to close Milestone 5, likely 2 issues if we want both local backup/restore and a small reporting/counts slice.
>
> Review editing and tag management are still open questions, not committed Milestone 5 requirements. I would not count them as required unless you decide they belong before closeout.

User:

> and a high level, what comes after milestone 5 , when do we have to get real data

Assistant summary:

> High level: after Milestone 5, the next sensible phase is real market/context data integration, but still read-only.
>
> The internal workflow is already in place:
>
> `idea -> thesis -> plan -> rules -> order intent -> position -> fills -> review -> journal export`
>
> Milestone 5 makes the review side usable. After that, the biggest missing input is external reality: prices, candles, volume, market regime, sector/index context, and maybe news/events later.
>
> The next milestone could be framed as Milestone 6: Read-Only Market Data Provider Integration.
>
> Likely scope:
>
> - Add an ADR for provider boundaries.
> - Implement a first provider, probably `yfinance` or another simple source.
> - Import/cache daily OHLCV snapshots for instruments.
> - Keep data advisory and non-canonical.
> - Link market snapshots to plans, positions, and reviews.
> - Support explicit commands like `fetch-context`, `fetch-price-history`, or `snapshot-instrument`.
> - No live streaming, broker integration, signals, automation, or trading recommendations yet.
>
> Suggested order:
>
> 1. Finish Milestone 5 with local ops / backup and maybe tiny summary reports.
> 2. Add provider ADR.
> 3. Implement read-only historical/daily market data ingestion.
> 4. Build simple comparison/reporting against completed trades.
> 5. Only later consider simulator, AI review assistance, or broker integration.

User:

> great everything is going per plan. save this as a note in raw directory if needed or as a restart prompt. after that will take a break

## Observations

- Milestone 5 has three completed slices: review tags/filtering, review quality scores, and Markdown journal export.
- Milestone 5 does not currently have a fixed remaining issue count.
- The likely minimum closeout path is one local operations issue for backup/restore or maintenance of the local JSON workflow.
- A second optional closeout issue could add narrow journal-grade counts or grouped summaries for completed reviewed trades.
- Review editing and tag management remain open questions, not required closeout items.
- Real market data becomes important after Milestone 5 because the core internal workflow and review loop are now usable.
- The next phase should still treat real data as read-only context, not as canonical trade meaning or execution automation.

## Ideas

- Close Milestone 5 with:
  - local backup/restore or export/restore guidance for JSON persistence
  - optionally a narrow completed-review summary report
- Define a future Milestone 6 around read-only market data provider integration.
- Start Milestone 6 with an ADR for external provider boundaries before adding provider code.
- First provider could be `yfinance` or another simple historical/daily data source.
- First data product should likely be daily OHLCV snapshots or historical price windows for instruments, not live streaming.
- Explicit CLI commands could include `fetch-context`, `fetch-price-history`, or `snapshot-instrument`.

## Questions

- Should Milestone 5 close after local backup/restore alone, or should it also include narrow review summary counts?
- Should review editing or tag management remain deferred beyond Milestone 5?
- Which first real-data provider is acceptable for local use, reliability, licensing, and failure behavior?
- What exact market data shape is needed first: latest quote, daily OHLCV history, market regime snapshot, index/sector context, or news/events?

## Concerns

- Real data integration should not blur the source-of-truth boundary: external data is advisory context, not canonical trade meaning.
- Provider integration should not introduce live trading, broker execution, automated signals, or recommendations.
- Any provider should be behind a clear port and documented by ADR before implementation.
- Milestone 6 should avoid becoming broad analytics, portfolio optimization, or market-data infrastructure.

## Possible Next Outputs

- Issue candidate: close Milestone 5 with local JSON backup/restore or operational maintenance workflow.
- Issue candidate: add narrow completed-review summary counts.
- ADR candidate: external market data provider boundary and first provider choice.
- Topic page update: future Milestone 6 read-only market data provider integration.
- Restart prompt: resume after break by deciding whether Milestone 5 needs one or two closeout issues.
