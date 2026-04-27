---
title: Milestone 5 Markdown Journal Export
type: topic
status: active
tags: [trading-system, milestone-5, reviews, export, learning]
created: 2026-04-26
updated: 2026-04-26
---

# Milestone 5 Markdown Journal Export

The third Milestone 5 implementation slice adds a narrow Markdown export for reviewed trades.

The purpose is to make completed reviews portable and easy to inspect outside the CLI while preserving the same journal-grade boundary as review tags and quality scores.

## Implemented Behavior

The application repo now includes:

- `ReviewJournalExportService`
- `export-review-journal --output <path>`
- optional `--overwrite`
- reuse of existing review filters for rating, purpose, direction, repeated tags, quality scores, and sort order

The export writes one Markdown section per matching review.

Each section includes:

- review id and reviewed timestamp
- position id and trade plan id
- purpose and direction
- realized P&L
- rating, tags, and process/setup/execution/exit scores
- summary, what went well, and what went poorly
- lessons learned and follow-up actions
- linked market-context metadata

Market-context payloads are intentionally excluded. Full payload inspection remains isolated to `show-context`.

## File Behavior

The CLI export is explicit and local:

- the parent output directory must already exist
- existing output files are refused unless `--overwrite` is provided
- empty results report `No trade reviews found.`
- empty results do not create a file

## Boundary

This slice intentionally does not add:

- CSV export
- charts
- aggregate statistics
- backup/restore workflows
- review editing
- recommendations
- generated coaching
- AI or ML-driven review interpretation

The exported Markdown is factual journal data from existing records, not generated analysis.

## Validation

The implementation was verified in the application repo on 2026-04-26:

```text
Focused review/export suite: 49 passed
Full suite: 142 passed
```

The first focused test command required running `uv` outside the sandbox because the sandbox could not access the local `uv` cache.

## Related Pages

- [[milestone-5-review-tags-and-filtering]]
- [[milestone-5-review-quality-scores]]
- [[milestones-3-to-5-roadmap]]
- [[application-implementation-status]]
- [[trade-lifecycle-and-objects]]
- [[product-roadmap-and-learning-boundaries]]
