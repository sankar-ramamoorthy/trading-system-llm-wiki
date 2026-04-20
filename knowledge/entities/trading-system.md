---
title: Trading System
type: entity
status: active
tags: [trading-system, project]
created: 2026-04-19
updated: 2026-04-19
---

# Trading System

The trading system is a professional-grade personal system for structured discretionary trading. It is for personal use, not a product or SaaS platform.

## Scope

The system should support:

- long-term investing
- swing trading
- occasional tactical or day trades
- stocks, ETFs, and options

Commodities, FX, and crypto are out of scope for now except where accessed through stocks or ETFs.

## Purpose

The system exists to improve trading discipline, context awareness, and long-term maintainability. It should track trade intent, enforce rules, detect process drift, and help decide when a trade or watchlist setup no longer deserves attention.

It is not intended to be a black-box AI trader, a rigid indicator-only rules engine, or a premature automation project.

## Core Goals

- Track and manage trades with full context: thesis, timeframe, playbook, invalidation, rules, lifecycle, and review history.
- Enforce discipline through rule checks, violation detection, and orphan or unplanned trade detection.
- Identify and evaluate opportunities through watchlist monitoring and setup degradation detection.
- Use AI for knowledge organization, context comparison, contradiction detection, and structured judgment support.

## Related Pages

- [[architecture-overview]]
- [[canonical-domain-model]]
- [[trade-lifecycle-and-objects]]
- [[context-intelligence-layer]]
