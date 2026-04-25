# Repository Guidelines

## Project Structure & Module Organization

This repository is a Markdown-first knowledge base for the `trading-system` project, not the runtime application codebase.

- `adr/` contains architecture decision records. Current ADRs use YAML front matter followed by Obsidian-friendly Markdown.
- `knowledge/raw/` stores unprocessed notes, summaries, imports, and dated working context.
- `knowledge/processed/`, `knowledge/entities/`, `knowledge/topics/`, and `knowledge/index/` are intended for refined wiki content, canonical concept pages, topic syntheses, and navigation pages.
- `knowledge/outputs/` is for generated answers or temporary synthesized outputs.
- `prompts/` contains restart prompts and reusable context for future AI-assisted sessions.
- `PROJECT.md`, if present, is the short current-context entrypoint for active milestone focus and high-priority links; keep durable knowledge in the wiki pages.

Keep diagrams ASCII-only. Prefer `[[wikilinks]]` for internal knowledge references when creating durable wiki pages.

## Build, Test, and Development Commands

There is currently no build system, package manager, or automated test suite in this repository.

## Useful Local Commands (PowerShell)

- `Get-ChildItem -Recurse -File`  
  Lists all files in the repository.

- `Get-ChildItem -Recurse -File | Select-String "term"`  
  Searches for a term across all files.

- `Get-ChildItem DOCS -Recurse -File | Select-String "TradeIdea"`  
  Searches for a specific domain term in documentation.

- `Get-ChildItem -Recurse -Filter *.md`  
  Lists all Markdown files.

- `Get-Content path\to\file.md`  
  Reads a Markdown file.

Before adding scripts or tooling, document the reason and expected workflow in an ADR or a short design note.

## Coding Style & Naming Conventions

Most contributions are Markdown. Use concise headings, short paragraphs, and YAML front matter for durable documents where metadata matters.

Recommended front matter:

```yaml
---
title: Example Title
type: note
status: draft
tags: [trading-system]
created: YYYY-MM-DD
---
```

Use descriptive filenames. Dated raw notes may use patterns like `EOD Summary 20260418.md`; canonical pages should prefer stable lowercase names such as `domain-model.md` or `swing-trading-rules.md`.

## Testing Guidelines

Testing is currently review-based. Check Markdown rendering, front matter validity, internal links, and whether new content belongs in `raw`, `processed`, `entities`, `topics`, or `index`.

When editing established knowledge, preserve prior context unless explicitly superseded. Call out contradictions rather than silently deleting them.

## Commit & Pull Request Guidelines

No local Git history is available in this checkout, so no repository-specific commit convention can be inferred. Use clear, imperative commits such as `Add trading system knowledge guide` or `Update canonical entity model notes`.

Pull requests should include:

- A short summary of changed knowledge.
- Links to related issues or prompts.
- Notes on any renamed, moved, or superseded pages.
- Screenshots only when Markdown rendering or diagrams are relevant.

## Agent-Specific Instructions

Treat this repository as persistent project memory. Integrate new notes incrementally, maintain cross-links, and keep generated output separate from canonical knowledge unless the user asks to promote it.

Current milestone focus, active project state, and short-term priorities should not be expanded here. Keep those in `PROJECT.md` or the relevant pages under `knowledge/topics/`, and keep this file focused on durable repository rules and knowledge-ingestion behavior.

## Linked Application Repo

The active runtime codebase for this project is expected at `C:\Users\bosto\dockerstuff\trading-system`. This knowledge base should preserve the why behind that codebase: architecture decisions, canonical entity definitions, source-of-truth boundaries, rejected alternatives, and prompts for future implementation sessions.

When summarizing or promoting new raw notes, verify against the linked application repo README, active code, and project instructions when practical. If a raw note contradicts the application repo or existing canonical pages, call out the contradiction rather than silently replacing prior context.

The application repo should keep its own focused `AGENTS.md` for build, test, runtime, and coding instructions. This repository's `AGENTS.md` should remain focused on knowledge ingestion, Markdown conventions, cross-linking, and durable project memory.

## Primary External Sources

Treat these application repo paths as primary source material when synthesizing wiki pages:

- `C:\Users\bosto\dockerstuff\trading-system\DOCS\`
- `C:\Users\bosto\dockerstuff\trading-system\DOCS\ADR\`

The repo `DOCS/ADR/` folder preserves versioned decision history tied to code. This knowledge base should not blindly duplicate those ADRs; it should synthesize their current implications into canonical entity, topic, index, and knowledge ADR pages.

The application repo `README.md` is maintained in the application repo. If a raw note implies README changes, record the desired alignment in the knowledge base and leave the actual README update to a repo-focused coding session unless the user explicitly asks to edit that repository.
