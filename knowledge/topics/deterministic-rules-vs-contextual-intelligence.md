---
title: Deterministic Rules vs Contextual Intelligence
type: topic
status: active
tags: [trading-system, rules, ai, context]
created: 2026-04-19
updated: 2026-04-19
---

# Deterministic Rules vs Contextual Intelligence

The central architectural separation is between enforceable deterministic rules and advisory contextual intelligence.

## Deterministic Rules

Deterministic rules are hard, explicit, auditable, and enforceable.

Examples:

- maximum position size
- no swing entry within a defined earnings window
- liquidity thresholds
- allowed playbooks
- risk and exposure limits
- no averaging down unless the playbook allows it
- no new trade after a daily loss threshold

These rules protect discipline and capital. Overrides should require explicit reason capture.

## First Rule Set

For the first executable slice, use a deliberately small rule set:

- maximum risk per trade
- invalidation is required before plan approval
- position must originate from an approved plan
- actual risk must stay within plan tolerance
- no duplicate active positions per instrument, if useful early

Each rule should be represented by stored metadata and evaluated by plain Python code before any rule DSL is considered.

## Contextual Intelligence

Contextual intelligence is advisory, structured, and interpretive.

Examples:

- thesis weakening
- peer divergence
- sentiment deterioration
- macro backdrop becoming less supportive
- chart becoming distributive instead of constructive
- regime becoming hostile to a playbook

These signals should affect review priority, sizing suggestions, conviction, playbook eligibility, and the need for manual confirmation.

## Design Principle

Do not turn AI-generated interpretation into fake certainty. Context intelligence should produce structured assessments with relevance, confidence, evidence, and recommended review action.

## Related Pages

- [[context-intelligence-layer]]
- [[trade-lifecycle-and-objects]]
- [[architecture-overview]]
- [[first-vertical-slice]]
