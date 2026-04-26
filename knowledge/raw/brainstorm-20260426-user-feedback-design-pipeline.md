---
title: User Feedback Design Pipeline Brainstorm
type: brainstorm
status: raw
tags: [trading-system, brainstorm, user-feedback, design-process]
created: 2026-04-26
---

# User Feedback Design Pipeline Brainstorm

## Trigger

The project is currently a single-user system in active development. The user wants product development and repository design decisions to take observed user feedback into account, especially feedback from their own friction while using and developing the system.

## Raw Input

I want product development and design - basically the design decisions of the repo - to take into consideration user feedback.
Currently the system is a single-user system and I am the only user, and we are in the development phase.

I had a brainstorm session and here is a note about that.
I will first add my takeaway:

1. Need a skill to convert feedback notes to a structured format.
2. There is a sample in the note.
3. Maybe we need a playbook that goes:

```text
Analyze these feedback entries.
Cluster them into underlying problems.
Propose solution directions with tradeoffs.
```

Good - you've hit a real design question, not a tooling one.

And I'm going to push back on one instinct right away:

> Don't turn the llm-wiki into a "thing that has skills."

That sounds nice, but it leads to fuzzy behavior and loss of control. What you actually need is **a disciplined feedback -> design pipeline**, not a smarter wiki.

---

## First: what "user feedback" actually means in your system

Since you are the only user, feedback is not:

- surveys
- usability studies
- external reports

It is:

> **friction you experience while using the system**

Like today:

- CLI friction
- typing overhead
- mismatch between mental model and interface

That's *high-quality signal*, not anecdotal noise.

---

## Where that feedback should go (this is the key)

Right now, the correct place is:

```text
knowledge/raw/
```

Not:

- ADRs
- code
- processed/canonical pages

Because at this stage, it's still:

> **unstructured, unvalidated input**

---

## Step 1 - Capture feedback properly (don't skip this)

When you hit friction, don't just think:

> "UX is bad"

Write something like:

```markdown
---
type: feedback
source: self
context: CLI usage
date: 2026-04-25
---

## Situation
Tried to create trade idea via CLI.

## Friction
CLI requires flags but appears like positional args might work.

## Impact
Breaks flow, requires re-checking help, slows input.

## Hypothesis
Interface mismatch between expected and actual usage.

## Possible directions
- positional args
- interactive mode
- LLM adapter
```

This structure matters. Otherwise everything becomes vague later.

---

## Step 2 - Don't jump to solutions yet

Your instinct was:

> "maybe I need an LLM adapter"

That's **a solution**, not the problem.

The real problem is:

> **input friction + cognitive mismatch**

LLM is *one possible response*, not the answer.

---

## Step 3 - Use the llm-wiki for *analysis*, not "skills"

Instead of:

> "teach the wiki brainstorming"

Do this:

- collect multiple feedback notes
- then ask the LLM, with that context:

```text
Analyze these feedback entries.
Cluster them into underlying problems.
Propose solution directions with tradeoffs.
```

Now the LLM is:

> **a reasoning tool over your data**, not an autonomous agent

---

## Step 4 - Promote patterns, not individual feedback

If you see repeated themes:

- friction in CLI
- slow workflows
- too many commands

Then create something like:

```text
knowledge/topics/cli-ux-friction.md
```

Now you've moved from:

- isolated complaint -> structured design concern

---

## Step 5 - Only then consider design changes

At that point, you can ask:

> "What are valid solutions within my architecture?"

Examples:

- positional args (low risk)
- interactive CLI (medium)
- LLM adapter (higher complexity)

Now you're making a **design decision**, not reacting emotionally.

---

## Where the LLM actually fits (during development)

Not as a "brainstorming skill inside the wiki."

But as:

### 1. A thinking amplifier

- analyze feedback
- compare options
- simulate tradeoffs

### 2. A design assistant

- propose interfaces
- critique UX decisions
- stress-test ideas

### 3. A translation layer

- turn messy thoughts -> structured notes
- turn notes -> ADR drafts

---

## What you should NOT do

Don't:

- embed "LLM features" into the wiki itself
- treat the wiki as an agent
- skip structuring feedback
- jump straight to implementation ideas

---

## The clean workflow (this is your answer)

```text
You experience friction
        |
        v
Write structured feedback (raw/)
        |
        v
Cluster + analyze (LLM-assisted)
        |
        v
Promote to topic if recurring
        |
        v
Evaluate solutions
        |
        v
If justified -> ADR -> code
```

---

## Your specific example (today)

You observed:

> CLI UX friction

Correct handling:

1. Write feedback note.
2. Identify underlying issue:
   - input friction
   - mismatch of expectations
3. List solutions:
   - positional args
   - interactive mode
   - LLM adapter (bigger step)
4. Decide later - not immediately.

---

## The important mindset shift

You're not:

> "adding features based on feedback"

You're:

> **evolving a system based on observed friction patterns**

That's a much higher bar - and exactly what your project is aiming for.

---

## Final answer to your question

> Do I instruct the llm-wiki?
> Do I give it a brainstorming skill?

No.

Instead:

> **You structure your thinking in the wiki, and use the LLM to reason over it.**

The intelligence stays with *you*. The LLM just helps process it.

If you want next, I can:

- design a clean feedback note template for your repo
- show how to turn feedback -> ADR cleanly

This is exactly the kind of discipline that will make your system very strong over time.

## Observations

- The project needs a way to treat self-observed friction as legitimate user feedback without confusing it with canonical design knowledge.
- The current feedback source is the user's own development and usage experience, especially CLI workflow pain.
- Raw feedback belongs in `knowledge/raw/` until it has been clustered, validated, or promoted.
- The note distinguishes the underlying problem from early solution ideas: "input friction + cognitive mismatch" is the problem; an LLM adapter is only one possible response.
- The llm-wiki should remain a knowledge base, not an autonomous agent or feature surface.

## Ideas

- Create a structured feedback note format for future raw feedback entries.
- Use an LLM-assisted analysis prompt or playbook after multiple feedback entries exist:

```text
Analyze these feedback entries.
Cluster them into underlying problems.
Propose solution directions with tradeoffs.
```

- Promote recurring patterns into topic pages, such as `knowledge/topics/cli-ux-friction.md`.
- Consider solution directions only after feedback patterns are visible and framed as design problems.
- Keep the LLM role as reasoning support: analyzing feedback, comparing options, proposing interfaces, stress-testing ideas, and translating rough notes into structured artifacts.

## Questions

- Should feedback notes get their own durable raw-note template distinct from brainstorm notes?
- When is a single high-impact friction event enough to promote into a topic page, versus waiting for recurrence?
- Should the feedback analysis playbook live as a skill, a prompt, a topic note, or a reusable raw-note template?
- What minimum evidence should be required before feedback drives an ADR or application repo issue?

## Concerns

- Turning the wiki itself into a "thing that has skills" could blur boundaries and make behavior fuzzy.
- Jumping directly from friction to implementation may lead to reactive design instead of deliberate architecture.
- Treating early solution ideas as the problem could prematurely bias future design decisions.
- Over-promoting raw feedback into canonical pages could pollute durable knowledge with unvalidated impressions.

## Possible Next Outputs

- Feedback note template
- Feedback analysis playbook
- Topic page candidate: CLI UX friction
- ADR candidate: feedback-to-design decision pipeline
- No immediate application code change
