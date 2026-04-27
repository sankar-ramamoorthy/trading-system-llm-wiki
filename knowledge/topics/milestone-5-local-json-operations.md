---
title: Milestone 5 Local JSON Operations
type: topic
status: active
tags: [trading-system, milestone-5, local-ops, json-persistence]
created: 2026-04-27
updated: 2026-04-27
---

# Milestone 5 Local JSON Operations

The fourth Milestone 5 implementation slice adds explicit local operations for the configured JSON store.

The purpose is to make the local-first workflow safer to maintain without introducing a new persistence architecture or cloud operational model.

## Implemented Behavior

The application repo now includes JSON store helpers for:

- validating the configured store
- backing up the configured store
- restoring the configured store from a backup

The CLI exposes those operations as:

- `validate-store`
- `backup-store`
- `backup-store --output-dir <path>`
- `restore-store <backup-path> --overwrite`

The configured store remains `TRADING_SYSTEM_STORE_PATH` when set, or `.trading-system/store.json` by default.

## Backup Behavior

Backups are exact timestamped JSON copies of the configured store.

The default backup location is:

```text
.trading-system/backups
```

The command creates the output directory when needed and fails clearly when the configured store is missing or invalid.

## Restore Behavior

Restore validates the backup before replacing the configured store.

Existing stores require explicit overwrite consent:

```powershell
uv run trading-system restore-store .\.trading-system\backups\<backup-file>.json --overwrite
```

Restore is an operational command over the active local JSON backend. It does not make backup files canonical domain records.

## Boundary

This slice intentionally does not add:

- encryption
- compression
- scheduled backups
- cloud sync
- migrations
- Postgres backup support
- broader operational automation

The feature is local, explicit, and single-user oriented.

## Validation

The implementation note recorded successful validation in the application repo:

```text
Focused persistence/CLI/retrieval suite: 67 passed
Full suite: 156 passed
```

The first sandboxed `uv` test run failed because the sandbox could not access the local `uv` cache; the approved elevated rerun passed.

## Related Pages

- [[milestone-5-markdown-journal-export]]
- [[milestones-3-to-5-roadmap]]
- [[application-implementation-status]]
- [[development-workflow]]
- [[data-and-platform-strategy]]
