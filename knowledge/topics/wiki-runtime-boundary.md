---
title: Wiki-Runtime Boundary
type: topic
status: active
tags: [trading-system, knowledge-base, architecture, boundary, learning-loop]
created: 2026-04-26
updated: 2026-05-02
---

# Wiki-Runtime Boundary

Promoted from raw brainstorm `brainstorm-20260426-post-development-wiki-boundaries.md`.

This is foundational architecture: keeping the two systems distinct prevents drift, runtime instability, and polluted domain logic.

---

## Two Distinct Systems

### The Trading System (runtime, canonical)

```text
C:\Users\bosto\dockerstuff\trading-system
```

- Source of truth for trades
- Deterministic engine
- Auditable system of record

Answers: **"What happened, and what does it mean?"**

### The LLM Wiki (knowledge system)

```text
C:\Users\bosto\dockerstuff\knowledge-base\trading-system
```

- Source of truth for thinking
- Long-term memory
- Context for decisions and evolution

Answers: **"Why does this system exist this way? What have I learned? What should change?"**

---

## Core Principle

> The wiki is not part of the trading system. It is a parallel system that observes, explains, synthesizes, and guides — but does not execute and does not define runtime truth.

---

## Direction of Influence

```text
Trading System  -------->  LLM Wiki
  (events, decisions)       (interpretation, synthesis)

LLM Wiki  -------->  Trading System
  (design decisions, ADRs)   (implemented deliberately)
```

- Left → Right: automatic / continuous
- Right → Left: **intentional / manual**

That asymmetry is critical. The wiki must not influence the runtime dynamically.

---

## What Flows Trading System → Wiki

Raw material for learning:

- TradeReviews
- Patterns across trades
- Rule violations
- Execution behavior and mistakes

These land in `knowledge/raw/`, then get promoted through processed → topics/entities/ADRs.

## What Flows Wiki → Trading System

Rare and deliberate only:

- New rules (`rules_engine/`)
- Domain changes (`domain/`)
- Workflow changes (`services/`)
- Architecture decisions (ADRs)

This is where *thinking becomes structure*.

---

## Three Roles the Wiki Plays

**1. External brain** — stores reasoning that should not live in code, CLI, or domain objects.

**2. Anti-corruption layer** — keeps fuzzy reasoning, evolving ideas, and half-formed concepts out of the deterministic domain model, so the domain stays clean, explicit, and enforceable.

**3. Pre-processor for future features** — before something becomes a rule, a field, or a workflow step, it should exist in the wiki first.

---

## Failure Modes to Avoid

**Failure mode 1: Wiki becomes pseudo-runtime**
If the wiki is queried during execution or used to influence decisions dynamically, determinism and auditability break.

**Failure mode 2: Duplicating domain concepts**
If `TradePlan` meaning exists differently in code and wiki, domain drift occurs. `DOCS/domain-model.md` is canonical.

**Failure mode 3: Skipping promotion**
If insights stay forever in `knowledge/raw/` and never become rules or structure, the system does not evolve.

---

## The Strongest Version (Vision)

```text
[ Trading System ]
     v (facts)
[ LLM Wiki ]
     v (insight)
[ Decisions / ADRs ]
     v (implementation)
[ Trading System evolves ]
```

---

## Open Question

Should `TradeReview` outputs automatically feed into the wiki, or should capture remain manual? Automatic ingestion risks noise. Manual ingestion risks stagnation. This decision is deferred until enough completed trades exist to validate the approach.

---

## Related Pages

- [[knowledge-base-workflow]]
- [[feedback-to-design-pipeline]]
- [[application-repo-documentation-sources]]
