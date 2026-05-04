---
title: CLI UX and Natural Language Mode Brainstorm
type: brainstorm
status: processed
tags: [trading-system, brainstorm, cli-ux, natural-language-input, llm-adapter]
created: 2026-04-26
---

# CLI UX and Natural Language Mode Brainstorm

## Trigger

The user hit friction while trying to create a trade idea through the CLI. The command looked like it might accept positional arguments, but the CLI required named flags, creating a mismatch between the user's mental model and the actual interface.

## Raw Input

Yeah, your CLI is technically working - but the UX is fighting the user a bit.

Right now it *looks* like positional arguments might work:

```text
trading-system create-trade-idea IGV IGV/SPY undervaluedIGV "3 weeks"
```

...but the tool actually *requires named flags*, so the user gets slapped with:

```text
Missing option '--instrument-id'
```

That's a classic UX mismatch: **the interface suggests one mental model but enforces another**.

---

## What's going wrong (UX-wise)

### 1. Mixed signals (biggest issue)

Your help output:

```text
Usage: trading-system create-trade-idea [OPTIONS]
```

This strongly implies:

> "Everything must be passed as flags"

But your actual usage attempt shows:

> "It feels like positional args should work"

So users guess wrong -> friction.

---

### 2. Too verbose for frequent actions

Typing this every time:

```text
--instrument-id IGV --playbook-id IGV/SPY --purpose undervaluedIGV --direction long --horizon "3 weeks"
```

...is painful for something you'll run often.

---

### 3. No affordance for discovery

There's no guidance like:

- examples
- defaults
- constrained values (`direction` should probably be enum)

---

## How to fix it (practical improvements)

### Option A - Support positional args (best UX upgrade)

Let users do this:

```text
trading-system create-trade-idea IGV IGV/SPY undervaluedIGV long "3 weeks"
```

Then internally map:

| Position | Meaning       |
| -------- | ------------- |
| 1        | instrument-id |
| 2        | playbook-id   |
| 3        | purpose       |
| 4        | direction     |
| 5        | horizon       |

Still allow flags for power users.

This is what tools like `git`, `docker`, `uv`, etc. do well.

---

### Option B - Hybrid (very strong pattern)

Allow **both**:

```text
# Fast path
trading-system create-trade-idea IGV IGV/SPY undervaluedIGV long 3w

# Explicit path
trading-system create-trade-idea \
  --instrument-id IGV \
  --playbook-id IGV/SPY \
  --purpose undervaluedIGV \
  --direction long \
  --horizon "3 weeks"
```

---

### Option C - Interactive mode (huge UX win)

If required args missing:

```text
Missing required options. Enter interactively:

Instrument ID: IGV
Playbook ID: IGV/SPY
Purpose: undervaluedIGV
Direction (long/short): long
Horizon: 3 weeks
```

This is *massively underrated* for internal tools.

---

### Option D - Better help output

Instead of:

```text
Usage: trading-system create-trade-idea [OPTIONS]
```

Do:

```text
Usage:
  trading-system create-trade-idea [ARGS] [OPTIONS]

Args:
  instrument-id
  playbook-id
  purpose
  direction
  horizon

Example:
  trading-system create-trade-idea IGV IGV/SPY undervaluedIGV long 3w

Or:
  trading-system create-trade-idea --instrument-id IGV ...
```

---

### Option E - Improve error messages

Instead of:

```text
Missing option '--instrument-id'
```

Do:

```text
Missing required input: instrument-id

Tip:
  You can pass it as:
    trading-system create-trade-idea IGV ...
  or:
    --instrument-id IGV
```

---

### Option F - Constrain inputs (small but powerful)

For example:

- `direction`: `long | short`
- `horizon`: parse `3w`, `5d`, etc.

This reduces ambiguity and improves confidence.

---

## If you're using Typer / Click

You probably defined everything as:

```python
def create_trade_idea(
    instrument_id: str = typer.Option(...),
    ...
)
```

Switch to:

```python
def create_trade_idea(
    instrument_id: str,
    playbook_id: str,
    purpose: str,
    direction: str,
    horizon: str,
):
```

-> these become positional automatically.

Then optionally add flags as overrides.

---

## Big picture UX principle

Your tool is for **you (or traders)** -> optimize for:

- speed
- muscle memory
- minimal typing
- low cognitive load

Not strict correctness via flags.

---

wondering if it would be better if an llm intercepts the input and accepts plain english

---

Short answer: **yes - but not as a replacement for your CLI, as a layer on top of it.**

If you go "LLM-only," you'll create new problems that are *worse* than your current UX. But if you **add an LLM as a translator**, you get the best of both worlds.

---

## The trap to avoid

It's tempting to think:

> "Why not just let users type:
> 'buy IGV vs SPY because it's undervalued over 3 weeks'"

The problem:

- ambiguous inputs -> inconsistent parsing
- hard to debug
- dangerous for trading (misinterpretation = real cost)
- not reproducible (important for logs/backtesting)

So don't replace structure - **wrap it**.

---

## The winning pattern (used by good tools)

Think of your system like this:

```text
User input (English)
        |
        v
LLM parser (structured output)
        |
        v
Strict CLI / core system (validated)
        |
        v
Execution / persistence
```

The CLI remains the **source of truth**.

---

## What this looks like in practice

### User experience

```text
trading-system "IGV looks undervalued vs SPY, go long for 3 weeks"
```

### LLM converts to:

```json
{
  "instrument_id": "IGV",
  "playbook_id": "IGV/SPY",
  "purpose": "undervalued",
  "direction": "long",
  "horizon": "3 weeks"
}
```

Then your existing command runs internally.

---

## Even better: show the interpretation (critical)

Never silently execute. Always confirm:

```text
Interpreted trade idea:

Instrument: IGV
Playbook: IGV/SPY
Direction: long
Horizon: 3 weeks
Purpose: undervalued

Proceed? (y/n/edit)
```

This solves:

- trust
- correctness
- debugging

---

## Where LLMs shine in your system

### 1. Input parsing (best use)

Turn messy text into structured fields.

### 2. Autofill missing fields

User says:

```text
IGV looks cheap vs SPY
```

LLM asks:

```text
What's your time horizon?
Direction (long/short)?
```

---

### 3. Smart defaults

- infer `direction = long` from "undervalued"
- infer `playbook = IGV/SPY` from context

---

### 4. Naming & journaling

Generate:

- cleaner `purpose`
- trade descriptions
- tags

---

## Where NOT to rely on LLMs

- final execution logic
- validation (always deterministic)
- anything involving money without confirmation

---

## Hybrid UX (this is the sweet spot)

You support **three modes**:

### 1. Power user (fast CLI)

```text
trading-system create-trade-idea IGV IGV/SPY undervalued long 3w
```

---

### 2. Assisted (LLM-enhanced)

```text
trading-system "IGV undervalued vs SPY over next 3 weeks"
```

---

### 3. Interactive fallback

```text
trading-system create-trade-idea
```

Prompts user step-by-step.

---

## Implementation idea (simple architecture)

You don't need anything fancy:

```python
def parse_with_llm(text: str) -> dict:
    # call LLM with schema
    return structured_output

def create_trade_idea_command(input: str):
    if looks_like_natural_language(input):
        data = parse_with_llm(input)
    else:
        data = parse_cli_args(input)

    validated = validate(data)
    confirm(validated)
    save(validated)
```

---

## One important pushback

If your system is **just for you**, LLM input is great.

If it's for:

- teams
- automation
- backtesting pipelines

You still need:

- strict schemas
- deterministic inputs

So don't lose that foundation.

---

## My recommendation (clear direction)

Do this:

1. Fix CLI UX (positional args + better errors)
2. Add LLM "natural language mode"
3. Always show structured confirmation
4. Don't replace your core with LLM parsing

---

If you want, I can design a really clean command interface like:

```text
trading-system idea ...
trading-system log ...
trading-system review ...
```

with LLM deeply integrated but still safe.

Looks like you might be pausing or thinking - so I'll push things forward a bit with something concrete.

If you *do* want to explore the LLM route, here's a **minimal, high-impact version** you could add in like ~1-2 hours that immediately upgrades UX without breaking anything.

---

## Drop-in "natural language mode"

### Step 1: Detect free text

If the user runs:

```bash
trading-system "IGV looks undervalued vs SPY over 3 weeks"
```

You detect:

- no flags
- single string input
  -> route to LLM

---

### Step 2: Force structured output (this is key)

Don't just ask the LLM loosely - give it a strict schema:

```python
schema = {
    "instrument_id": "string",
    "playbook_id": "string",
    "purpose": "string",
    "direction": "long | short",
    "horizon": "string"
}
```

Prompt like:

```text
Extract a trade idea into this JSON schema.
Be strict. If unknown, return null.

Text:
"IGV looks undervalued vs SPY over 3 weeks"
```

---

### Step 3: Validate like your life depends on it

```python
if data["direction"] not in ["long", "short"]:
    raise ValueError("Invalid direction")

if not data["instrument_id"]:
    ask_user("What instrument?")
```

---

### Step 4: Show before executing (non-negotiable)

```bash
Interpreted:

Instrument: IGV
Playbook: IGV/SPY
Direction: long
Horizon: 3 weeks
Purpose: undervalued

Confirm? (y/n)
```

---

## Why this works really well

- feels "AI-native"
- keeps your system deterministic
- avoids silent mistakes
- faster than flags
- safer than pure LLM

---

## The real UX upgrade (subtle but powerful)

Once this exists, your tool becomes:

> "I can think in English, but still operate with precision."

That's a *huge* leap in usability.

---

## If you're unsure whether it's worth it

Ask yourself:

- Do you often hesitate typing flags? -> LLM helps
- Do you repeat similar trade ideas? -> LLM helps
- Do you care about correctness/logging? -> keep CLI core

If all 3 = yes -> this is a strong upgrade.

## Observations

- The current CLI is functionally correct but creates UX friction for frequent trade idea creation.
- The command shape suggests positional arguments might work, but the implementation requires named flags.
- The error message exposes the implementation requirement without helping the user recover.
- The workflow is too verbose for repeated personal trading-system use.
- The note separates two levels of improvement: conventional CLI ergonomics and an LLM-assisted natural-language layer.

## Ideas

- Add positional arguments for the common `create-trade-idea` path while preserving named flags for explicit use.
- Improve command help with examples, argument descriptions, and flag-based alternatives.
- Improve error messages with recovery tips and valid invocation examples.
- Add an interactive fallback when required inputs are missing.
- Constrain fields such as `direction` and parse compact horizons such as `3w` or `5d`.
- Add a natural-language mode where the LLM translates user text into a strict structured schema.
- Always show the interpreted structured trade idea for confirmation before saving or executing anything.
- Keep deterministic CLI parsing, validation, persistence, and execution as the source of truth.
- Support three usage modes: fast CLI, LLM-assisted natural language, and interactive fallback.

## Questions

- Should positional arguments be added before any LLM-assisted input mode?
- What is the minimal command surface for a cleaner CLI, such as `trading-system idea`, `log`, and `review`?
- Which fields should be required for the initial natural-language parser?
- Should the LLM adapter ask follow-up questions for missing fields or require confirmation with null values?
- What confirmation workflow is enough to preserve auditability and avoid silent misinterpretation?
- Should this become an application issue, an ADR, or remain a raw UX brainstorm until more CLI friction appears?

## Concerns

- An LLM-only interface could create ambiguous parsing, inconsistent behavior, and hard-to-debug trade records.
- Silent execution from natural language would be unsafe for a trading workflow.
- Over-optimizing for AI input could weaken deterministic schemas needed for automation, backtesting, and auditability.
- Adding multiple input modes could complicate the CLI if the command hierarchy is not kept simple.
- Raw UX ideas should not bypass the promotion rule into implementation without application repo alignment.

## Possible Next Outputs

- Issue candidate: improve `create-trade-idea` CLI ergonomics
- Issue candidate: add natural-language trade idea adapter
- Topic page update: CLI UX friction
- ADR candidate: LLM input adapter boundary
- No action
