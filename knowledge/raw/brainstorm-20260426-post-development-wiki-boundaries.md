---
title: Post-Development Wiki Usage and Runtime Boundary Brainstorm
type: brainstorm
status: processed
tags: [trading-system, brainstorm, llm-wiki, runtime-boundary, learning-loop]
created: 2026-04-26
---

# Post-Development Wiki Usage and Runtime Boundary Brainstorm

## Trigger

The note explores how the LLM wiki should be understood after development, especially once the trading system is producing runtime facts and the wiki becomes a learning and interpretation layer.

## Raw Input

Here's how I understand your **llm-wiki (knowledge base)** in the big picture - and I'm going to be precise, because this piece can either become a massive advantage or a source of architectural confusion.

---

## The clean mental model

Your system is actually **two distinct but connected systems**:

### 1. The trading system (runtime, canonical)

Located at:

```text
C:\Users\bosto\dockerstuff\trading-system
```

This is your:

- **source of truth for trades**
- **deterministic engine**
- **auditable system of record**

It answers:

> "What happened, and what does it mean?"

---

### 2. The llm-wiki (knowledge system)

Located at:

```text
C:\Users\bosto\dockerstuff\knowledge-base\trading-system
```

This is your:

- **source of truth for thinking**
- **long-term memory**
- **context for decisions and evolution**

It answers:

> "Why does this system exist this way?"
> "What have I learned?"
> "What should change?"

---

## The key insight (this is the core)

> The llm-wiki is **not part of the trading system**.

It is a **parallel system** that:

- observes
- explains
- synthesizes
- guides

But does **not execute** and does **not define runtime truth**.

---

## How they integrate (correctly)

Think in terms of **direction of influence**:

```text
Trading System  ------->  llm-wiki
     (events, decisions)     (interpretation, synthesis)

llm-wiki -------> Trading System
  (design decisions, ADRs, ideas)   (implemented deliberately)
```

### Important:

- Left -> Right = **automatic / continuous**
- Right -> Left = **intentional / manual**

That asymmetry is critical.

---

## What flows from trading system -> llm-wiki

This is your **raw material for learning**:

- TradeReviews
- Patterns across trades
- Rule violations
- Execution behavior
- Mistakes and deviations

These should land in:

- `knowledge/raw/` (initial capture)
- then promoted to:
  - `entities/` (e.g., playbooks, behaviors)
  - `topics/` (e.g., "late exits", "overtrading")
  - `processed/` (cleaned insights)

---

## What flows from llm-wiki -> trading system

This should be **rare and deliberate**:

- new rules (-> `rules_engine/`)
- domain changes (-> `domain/`)
- workflow changes (-> `services/`)
- architecture decisions (-> ADRs)

This is where:

> thinking becomes structure

---

## What the llm-wiki is REALLY doing

It's acting as your:

### 1. External brain

- stores reasoning you don't want in code
- keeps long-form thinking out of CLI / domain

---

### 2. Anti-corruption layer (this is subtle but important)

Instead of polluting your domain with:

- fuzzy reasoning
- evolving ideas
- half-formed concepts

...you put them in the wiki.

So your domain stays:

- clean
- explicit
- enforceable

---

### 3. Pre-processor for future features

Before something becomes:

- a rule
- a field
- a workflow step

...it should exist in the wiki first.

---

## Where things could go wrong (important)

This setup is powerful, but fragile if boundaries blur.

### Failure mode 1: wiki becomes pseudo-runtime

If you start:

- querying it during execution
- using it to influence decisions dynamically

You break:

> determinism + auditability

---

### Failure mode 2: duplicating domain concepts

If `TradePlan` meaning exists in:

- code **and**
- wiki (differently)

You create drift.

Your AGENTS.md already guards against this:

> domain-model.md is canonical

Good.

---

### Failure mode 3: skipping promotion

If insights stay forever in:

- `raw/`

...and never become:

- rules
- structure

Then the system doesn't evolve.

---

## The strongest version of this system (vision)

If done right, you get:

```text
[ Trading System ]
   v (facts)
[ llm-wiki ]
   v (insight)
[ Decisions / ADRs ]
   v (implementation)
[ Trading System evolves ]
```

That loop is extremely powerful.

---

## My blunt assessment

You've unintentionally built something quite sophisticated:

> A **closed-loop learning system for discretionary trading**

Most traders either:

- journal loosely (no structure), or
- automate blindly (no reflection)

You're doing neither.

---

## One suggestion (not a change, just framing)

You might want to explicitly define this in your mental model:

> **The trading system captures reality.
> The llm-wiki interprets reality.**

If you keep that line sharp, you won't get into trouble later.

---

## Where I'd push you (gently)

At some point (not now), you'll want to decide:

- Do TradeReviews automatically feed into the wiki?
- Or is promotion always manual?

That decision affects:

- noise vs signal
- system complexity
- how much you trust automation in your thinking loop

---

Whenever you're ready, your "can of worms" is probably going to sit somewhere in this boundary.

Go ahead.

## Observations

- The note defines two distinct systems: the runtime trading system and the LLM wiki.
- The trading system is framed as the canonical runtime source of truth for trades, deterministic meaning, auditability, and system-of-record behavior.
- The LLM wiki is framed as a parallel knowledge system for thinking, interpretation, synthesis, learning, and future evolution.
- The integration is intentionally asymmetric: runtime facts may flow into the wiki continuously, while wiki insights should flow back into the trading system only through deliberate design decisions.
- The wiki's post-development role differs from its development role: it becomes downstream of runtime behavior as a learning and interpretation layer.

## Ideas

- Treat the wiki as an external brain for long-form reasoning that should not live in code, CLI flows, or domain objects.
- Use the wiki as an anti-corruption layer that keeps fuzzy reasoning, evolving ideas, and half-formed concepts out of the deterministic domain model.
- Let runtime artifacts such as `TradeReview` outputs, rule violations, execution behavior, mistakes, and deviations become raw material for learning.
- Promote observed runtime patterns from `knowledge/raw/` into topics, entities, ADRs, or application changes only after synthesis.
- Preserve a simple framing: the trading system captures reality; the LLM wiki interprets reality.

## Questions

- Should `TradeReview` outputs automatically feed into the wiki, or should capture remain manual?
- If automatic capture is added later, what is the threshold for noise control?
- Which runtime artifacts should be eligible for wiki ingestion first?
- When should an observed pattern become a new rule, workflow change, domain change, or ADR?
- How should the wiki avoid duplicating canonical domain concepts differently from the application repo?

## Concerns

- The wiki could become pseudo-runtime if it is queried during execution or used to influence trading decisions dynamically.
- Runtime determinism and auditability could be weakened if wiki interpretation leaks into execution paths.
- Domain drift could occur if concepts such as `TradePlan` are defined differently in code and wiki pages.
- Learning could stall if runtime insights remain in `knowledge/raw/` and never become structured knowledge, ADRs, rules, or workflow changes.
- Automatic ingestion could create noise if it captures too much without a promotion or review discipline.

## Possible Next Outputs

- Topic page update: post-development wiki boundary
- Topic page update: closed-loop learning workflow
- ADR candidate: runtime-to-wiki ingestion boundary
- ADR candidate: automatic versus manual TradeReview ingestion
- No action
