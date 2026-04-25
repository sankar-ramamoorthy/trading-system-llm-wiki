do i need a project.md in the llm-wiki to avoid AGENT.md bloat

Short answer: **yes—but only if you keep it very intentional.**
Otherwise you’ll just move the bloat from `AGENTS.md` into another file and gain nothing.

---

## The real problem you're sensing

Your `AGENTS.md` is trying to do *too many jobs*:

* rules for behavior ✅
* architecture constraints ✅
* workflow guidance ⚠️
* evolving thinking ❌ (this is where bloat starts)

So the question isn’t:

> “Do I need `project.md`?”

It’s:

> **“How do I separate stable rules from evolving context?”**

---

## Clean separation (this is what you want)

### 1. `AGENTS.md` → **hard constraints only**

This should be:

> “If violated, the system breaks”

Examples:

* architecture boundaries
* domain invariants
* forbidden patterns
* repo structure rules

It should feel like:

> a contract, not a guide

---

### 2. `project.md` → **working context**

This becomes:

> “What are we doing right now and why?”

Contents:

* current milestone focus
* active concerns (like your CLI UX issue)
* open questions
* short-term priorities
* links to relevant notes (brainstorms, topics)

---

## What goes where (very concrete)

### Keep in `AGENTS.md`

✔ “domain must not depend on infrastructure”
✔ “do not collapse TradeIdea / TradePlan”
✔ “no code without issue”

---

### Move OUT of `AGENTS.md`

Anything like:

* “we are currently focusing on…”
* “next milestone is…”
* “this might evolve…”
* “consider doing X later…”

That belongs in `project.md`.

---

## What `project.md` should look like

Keep it tight. Example:

```markdown
---
type: project
status: active
---

## Current Phase
Milestone 4 – Read-only market context

## Active Focus
- Introduce context without breaking canonical boundaries
- Maintain strict separation between external data and trade meaning

## Current Concerns
- CLI UX friction (input ergonomics)
- Whether to introduce LLM adapter as interface layer

## Open Questions
- How should user feedback be captured and promoted?
- What is the correct boundary for natural language input?

## Active Threads
- [[cli-ux-improvement]] (topic)
- [[brainstorm-cli-input-2026-04-25]]

## Notes
- Avoid introducing non-determinism into core workflow
- Preserve auditability at all costs
```

---

## Why this works

### 1. Keeps AGENTS.md small and enforceable

Agents (and you) can actually follow it.

---

### 2. Gives you a “control panel”

Instead of digging through:

* raw notes
* ADRs
* docs

You have one place that says:

> “Here’s what matters *right now*”

---

### 3. Works perfectly with your workflow

```text
brainstorm → raw → topic → (maybe ADR)
                ↑
           project.md links here
```

---

## What NOT to do

### ❌ Don’t turn `project.md` into:

* another README
* a long narrative
* a dumping ground

If it grows beyond ~1 screen, it’s already drifting.

---

## Subtle but important point

`project.md` is:

> **stateful (changes often)**

`AGENTS.md` is:

> **stable (changes rarely)**

If you mix those, you get confusion and drift.

---

## My recommendation (clear)

Yes—add `project.md`, but:

* keep it short
* update it frequently
* use it as a *navigation + focus layer*, not documentation

---

## The deeper pattern you’re building

Without explicitly naming it, you now have:

* **AGENTS.md → rules**
* **project.md → current state**
* **wiki → thinking + knowledge**
* **repo → execution**

That’s actually a very clean system.

---


* define update rules for `project.md` (when to touch it)
