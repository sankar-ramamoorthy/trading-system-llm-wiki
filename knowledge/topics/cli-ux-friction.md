---
title: CLI UX Friction
type: topic
status: active
tags: [trading-system, cli, ux, friction, natural-language, milestone-9]
created: 2026-04-26
updated: 2026-05-02
---

# CLI UX Friction

Promoted from:
- `brainstorm-20260426-cli-ux-natural-language-mode.md`
- `brain storm on UX and ui.md`

This is a living topic. Add friction observations here as they occur rather than in raw notes.

---

## Current Friction Patterns

### 1. UUID requirement for instrument and playbook

The original CLI required `--instrument-id` and `--playbook-id` as raw UUIDs. Seeded symbols and slugs (e.g., `NVDA`, `pullback-to-trend`) are how a trader thinks, not internal IDs.

**Status: partially resolved in Milestone 8.** `create-trade-idea` now accepts `--symbol NVDA` and `--playbook-slug pullback-to-trend`. `fetch-market-data` and `fetch-options-chain` also auto-resolve from symbol.

### 2. Flags-only interface for frequent actions

Typing `--instrument-id ... --playbook-slug ... --purpose ... --direction ... --horizon ...` on every trade idea creation is verbose for a tool used repeatedly.

**Status: open.** The `--symbol` / `--playbook-slug` shortcuts reduce one dimension of friction but the command remains flag-heavy.

### 3. No examples in help output

`Usage: trading-system create-trade-idea [OPTIONS]` gives no guidance on what values are valid or what a typical invocation looks like.

**Status: open.**

### 4. Enum fields not constrained

`direction` should only accept `long` or `short`. `horizon` should parse compact values like `3w` or `5d`. Currently accepts any string, which reduces confidence.

**Status: open.**

---

## Proposed Improvements (not yet accepted as issues)

### A: Positional arguments for common commands

Allow the fast path:
```text
trading-system create-trade-idea NVDA pullback-to-trend swing long days_to_weeks
```
Still allow named flags for explicit use. This mirrors how `git`, `docker`, and `uv` work.

### B: Interactive fallback when required inputs are missing

If required fields are omitted:
```text
Missing required inputs. Enter interactively:
Symbol: NVDA
Playbook: pullback-to-trend
Direction (long/short): long
Horizon: days_to_weeks
Purpose: swing
```
This is highly effective for internal tools used by a single trader.

### C: Better error messages with recovery tips

Instead of `Missing option '--instrument-id'`, show:
```text
Missing required input: instrument

Tip:
  trading-system create-trade-idea --symbol NVDA ...
```

### D: Constrained enum inputs

Accept only `long | short` for direction. Parse `3w`, `5d`, etc. for horizon.

---

## Natural Language Mode (future, not yet accepted)

The larger vision: add an LLM layer on top of the CLI that translates trader language into structured commands.

```text
User: "NVDA looks strong, going long on a pullback-to-trend over the next few weeks"
                |
                v
LLM parser (structured output)
                |
                v
Strict CLI (validated, confirmed by user)
                |
                v
Execution / persistence
```

**Key rules for any natural language mode:**
- Never silently execute — always show the interpreted structure for confirmation
- LLM translates; CLI validates and persists
- Three modes: fast CLI, LLM-assisted, interactive fallback
- Keep deterministic core as source of truth; LLM is a convenience layer only

**Status: ADR candidate.** Not yet accepted as implementation scope. Candidate for Milestone 9 or later alongside web product work.

---

## Guardrails

- Do not replace the deterministic CLI core with LLM-only input
- Do not silently execute natural language — confirmation is non-negotiable
- Do not let natural language input bypass validation or audit trail
- LLM input is appropriate for single-user personal tools; strict schemas remain required for automation or backtesting

---

## Related Pages

- [[feedback-to-design-pipeline]]
- [[api-first-trade-capture-product-vision]]
- [[wiki-runtime-boundary]]
