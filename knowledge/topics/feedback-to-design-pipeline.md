---
title: Feedback-to-Design Pipeline
type: topic
status: active
tags: [trading-system, knowledge-base, process, feedback, design-decisions]
created: 2026-04-26
updated: 2026-05-02
---

# Feedback-to-Design Pipeline

Promoted from raw brainstorm `brainstorm-20260426-user-feedback-design-pipeline.md`.

Describes how self-observed friction converts into deliberate architecture — not reactive feature additions.

---

## What User Feedback Means Here

Since this is a single-user system, feedback is not surveys or usability studies. It is:

> **Friction you experience while using the system.**

CLI friction, typing overhead, mental model mismatches — these are high-quality signal, not anecdotal noise.

---

## The Pipeline

```text
You experience friction
        |
        v
Write structured feedback (raw/)
        |
        v
Cluster + analyze (LLM-assisted after several entries)
        |
        v
Promote to topic if recurring
        |
        v
Evaluate solutions
        |
        v
If justified → ADR → code
```

---

## Step 1: Capture Friction Properly

Use this structure in `knowledge/raw/`:

```markdown
---
type: feedback
source: self
context: CLI usage
date: YYYY-MM-DD
---

## Situation
What were you trying to do?

## Friction
What specifically broke your flow?

## Impact
Speed, correctness, confidence?

## Hypothesis
What's the underlying mismatch?

## Possible directions
- option A
- option B
```

Do not jump straight to solutions. Capture the friction first.

---

## Step 2: Don't Jump to Solutions

Early solution ideas are symptoms, not answers. "I need an LLM adapter" is a solution. "Input friction + cognitive mismatch" is the problem. Address the problem; evaluate solutions separately.

---

## Step 3: Use LLM for Analysis, Not Features

After collecting multiple feedback notes, ask:

```text
Analyze these feedback entries.
Cluster them into underlying problems.
Propose solution directions with tradeoffs.
```

The LLM is a reasoning tool over your data — not an autonomous agent.

---

## Step 4: Promote Patterns, Not Individual Complaints

If the same theme recurs (e.g., CLI verbosity, UUID friction, slow workflows), promote it:

```text
knowledge/topics/cli-ux-friction.md
```

Individual complaints stay in raw. Patterns become durable topics.

---

## Step 5: Only Then Consider Design Changes

Once a pattern is promoted, evaluate:

- What are valid solutions within the current architecture?
- What is the risk and reversibility of each?
- Does this warrant an ADR?

---

## What NOT to Do

- Do not embed "LLM features" into the wiki itself
- Do not treat the wiki as an autonomous agent
- Do not skip structuring feedback — vague notes create vague solutions
- Do not jump straight to implementation from a single friction event

---

## The Mindset Shift

You are not "adding features based on feedback." You are **evolving a system based on observed friction patterns.** That is a much higher bar, and the correct one.

---

## Related Pages

- [[wiki-runtime-boundary]]
- [[cli-ux-friction]]
- [[knowledge-base-workflow]]
