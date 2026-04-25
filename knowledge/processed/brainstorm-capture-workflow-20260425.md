How to capture brainstorming.md
Good—this is a much sharper question.

You’re not trying to make the wiki *think*.
You’re trying to **capture thinking without corrupting structure**.

That’s exactly the right concern.

---

## The core problem

Brainstorming is:

* messy
* nonlinear
* contradictory
* solution-heavy

Your system (and wiki) is:

* structured
* layered
* intentional

So the tension is:

> **How do I allow messy thinking without polluting clean knowledge?**

---

## The answer (clean model)

You don’t “fit brainstorming into the system.”

You **give it a containment layer**.

---

## Where brainstorming lives

You already have the right place:

```text
knowledge/raw/
```

But right now it’s just “raw notes.”

What you’re missing is:

> a **specific format for brainstorming sessions**

---

## Add a new note type: `brainstorm`

Not a tool. Not a feature. Just a convention.

---

### Example

```markdown
---
type: brainstorm
topic: cli-ux-improvement
date: 2026-04-25
status: messy
---

## Trigger
Friction when using CLI to create trade ideas.

## Observations
- Flags feel heavy
- Expected positional args
- Flow interruption when command fails

## Ideas (unfiltered)
- positional arguments
- interactive CLI mode
- LLM adapter for natural language
- command aliases / shortcuts
- pre-filled defaults

## Questions
- Is speed or clarity more important?
- Is this tool for daily high-frequency use?

## Concerns
- LLM breaks determinism?
- interactive mode slows power users?
- positional args reduce clarity?

## Rough directions
- Hybrid CLI (positional + flags)
- Optional interactive mode
- LLM as adapter only (not core)

## Status
Unresolved
```

---

## Why this works

It does **three important things**:

### 1. Preserves messiness

You don’t force structure too early.

---

### 2. Makes thinking explicit

Instead of:

> “I had some ideas”

You now have:

* trigger
* observations
* ideas
* concerns

---

### 3. Keeps it isolated

This does NOT:

* affect ADRs
* affect domain model
* affect code

---

## What happens next (this is the important part)

Brainstorm notes are **not permanent artifacts**.

They are **input material**.

---

## Promotion flow (lightweight)

From a brainstorm, you extract:

### → Feedback (if it’s about friction)

Move distilled insight to:

```text
knowledge/raw/ (feedback note)
```

---

### → Topic (if recurring)

```text
knowledge/topics/cli-ux.md
```

---

### → ADR (if decision is made)

Only when:

> “We are choosing X over Y because Z”

---

## What you should NOT do

### ❌ Don’t:

* clean up brainstorm notes too early
* merge them into “processed” content
* treat them as canonical
* over-structure them

---

## How the LLM fits into brainstorming

This is where it *actually* helps.

Not as a “brainstorming agent,” but as:

---

### 1. Expansion tool

You write:

```text
CLI is annoying
```

You ask:

```text
Expand this into possible UX problems and solution directions
```

---

### 2. Structuring tool

You dump messy thoughts, then ask:

```text
Organize this into observations, ideas, concerns
```

---

### 3. Critique tool

```text
What are the risks of these ideas given my architecture?
```

---

## The important discipline

Never skip this step:

> **You (not the LLM) decide what matters**

Otherwise:

* everything looks equally important
* noise becomes structure

---

## Minimal system change (what you actually need)

You don’t need new tooling.

Just:

### 1. A convention

```text
type: brainstorm
```

### 2. A habit

* capture when thinking, not after
* don’t filter too early

---

## The deeper insight

You’re building **two different cognitive modes**:

| Mode               | Where                       | Rules  |
| ------------------ | --------------------------- | ------ |
| Thinking (messy)   | `raw/brainstorm`            | loose  |
| Knowledge (stable) | `processed/entities/topics` | strict |

Most systems fail because they mix these.

You’re now explicitly separating them.

---

## If I push you one step further

At some point, you may want:

```text
knowledge/index/brainstorms.md
```

Just a simple index of active thinking threads.

Not necessary now—but useful later when you have many.

---

## Bottom line

You don’t need to change your system.

You just need to:

> **treat brainstorming as a first-class, but temporary, artifact**

---

If you want next, I can help you:

* design the exact promotion rules (brainstorm → topic → ADR)
* or show how to avoid “idea bloat” as these accumulate

This is a really solid evolution of your setup.
