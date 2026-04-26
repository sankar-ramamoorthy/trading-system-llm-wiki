---
name: structure-feedback-note
description: Convert messy user feedback notes, self-observed workflow friction, or pasted feedback into a structured raw feedback Markdown note for this knowledge base. Use when the user asks to structure, capture, convert, or normalize feedback notes into the feedback template, especially notes from knowledge/raw/ that should become clearer design input before any topic, ADR, or code change.
---

# Structure Feedback Note

## Purpose

Turn unstructured user feedback into a raw, non-canonical feedback note that can be analyzed later as design input.

## Workflow

1. Identify the target wiki root.
   - If the current workspace has `knowledge/raw/`, use it.
   - If no `knowledge/raw/` exists, ask for the target directory before writing.
2. Identify the source feedback note.
   - If the user names a file, use that file.
   - If the user only says "the feedback note" or similar, inspect `knowledge/raw/` for likely feedback notes.
   - If multiple likely notes exist, ask which one to convert.
3. Infer a concise title, topic slug, context, and tags from the source note.
4. Create one Markdown file in `knowledge/raw/`.
   - Use filename pattern: `feedback-YYYYMMDD-short-topic.md`.
   - Use the current local date for `created`.
5. Preserve the user's original thinking under `Raw Input`.
   - Lightly clean obvious copy/paste or encoding artifacts only when they obscure meaning.
   - Keep uncertainty, contradictions, rough language, and early solution ideas visible.
6. Organize the note into the required template below.
7. After successful conversion, move the original source note to `knowledge/processed/`.
   - Preserve the original filename when moving it.
   - If the destination filename already exists, ask before overwriting or renaming.
8. Stop after creating the structured feedback note and moving the original unless the user separately asks for analysis, promotion, issues, ADRs, or implementation.

## Required Note Shape

```markdown
---
title: Example Feedback Title
type: feedback
status: raw
source: self
context: Example context
tags: [trading-system, feedback, short-topic]
created: YYYY-MM-DD
---

# Example Feedback Title

## Situation

What the user was trying to do.

## Friction

What felt slow, confusing, mismatched, or painful.

## Impact

How it affected flow, confidence, speed, correctness, or decision quality.

## Hypothesis

The suspected underlying problem, without treating solutions as proven.

## Possible Directions

- Potential solution direction
- Alternative direction
- No action / observe more

## Raw Input

The user's original note, pasted output, or rough feedback.
```

## Classification Guidance

- Treat feedback as evidence of friction, not as an immediate requirement.
- Keep solution ideas in `Possible Directions`; do not present them as decisions.
- Use `Hypothesis` for the suspected underlying problem, such as input friction, cognitive mismatch, missing feedback loop, or workflow overhead.
- Prefer `source: self` when the user is reporting their own experience.
- Prefer a concrete `context`, such as `CLI usage`, `knowledge ingestion`, `planning workflow`, or `application development`.

## Boundaries

- Do not treat feedback notes as canonical knowledge.
- Do not update index pages, topic pages, entity pages, `PROJECT.md`, ADRs, or application repo files.
- Do not create GitHub issues, implementation plans, or design decisions unless the user explicitly asks.
- Do not over-resolve the feedback. Preserve the difference between observed friction, inferred problem, and possible solution.

## Defaults

- Prefer ASCII Markdown.
- Prefer `knowledge/raw/` rather than adding a feedback subdirectory.
- Prefer `knowledge/processed/` for moved originals.
- Use wikilinks only when an obvious existing page is named by the user or visible in the current context.
