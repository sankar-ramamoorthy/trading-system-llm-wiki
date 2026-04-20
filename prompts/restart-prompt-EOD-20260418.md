# B. Restart prompt for a fresh chat

Use this as the first message in the new session.

```text
I want to continue designing a professional-grade personal trading system for my own use.

Please treat the following as established project context and continue from it without re-litigating earlier decisions unless I explicitly ask.

Project name:
- trading-system

Project intent:
- This is a professional-grade personal trading system for structured discretionary trading.
- It is for my own use, not for sale.
- It must support long-term investing, swing trading, and occasional tactical/day trades.
- The emphasis is on rigor, discipline, context awareness, and long-term maintainability.
- I have trading experience and Python experience.

What this system is NOT:
- not a black-box AI trader
- not a rigid indicator-only rule engine
- not a premature automation project
- not a product/company SaaS architecture

Core goals:
1. Track and manage trades with full context
   - thesis
   - timeframe
   - playbook
   - invalidation
   - rules
   - lifecycle
   - review history
2. Enforce discipline
   - rule checks
   - violation detection
   - orphan/manual/unplanned trade detection
3. Identify and evaluate opportunities
   - watchlist monitoring
   - setup qualification/degradation detection
   - later AI/ML only where it adds real value

Key architectural direction:
- The system should be:
  structured discretionary trading + deterministic discipline + AI-assisted context awareness
- Use a modular monolith, not business microservices
- Keep strong internal module boundaries
- Postgres may be isolated as infrastructure
- Docker is acceptable pragmatically, but should not drive premature service decomposition
- Development/deployment approach: hybrid
  - app can run locally with uv during early development
  - Postgres can run in Docker
  - full app+db+worker containerization can come later if useful

Major architectural principle:
- Separate deterministic rules from contextual intelligence

Deterministic rules:
- hard constraints
- explicit
- auditable
- enforceable

Contextual intelligence:
- advisory
- structured
- interpretive
- used for thesis monitoring, change detection, regime assessment, peer/macro/filing context

High-level layers:
1. Deterministic Control Layer
2. Market & Context Observation Layer
3. Context Intelligence Layer
4. Decision Support Layer

Source-of-truth principle:
- The system owns meaning; external systems provide facts

System is source of truth for:
- trade ideas
- playbooks
- thesis
- plans
- position purpose
- lifecycle state
- rules and evaluations
- violations
- journals/reviews

External systems are source of truth for:
- brokers: orders, fills, balances, raw positions
- market data providers: bars, quotes, options metadata/chains
- context sources: filings, news, peer events, macro/policy events

Established canonical entity model:
Market identity:
- Instrument
- OptionContract
- Universe
- UniverseMembership

Opportunity and planning:
- Playbook
- TradeIdea
- TradeThesis
- TradePlan
- WatchlistItem

Position and execution:
- Position
- PositionLot
- OrderIntent
- BrokerOrder
- Fill
- BrokerAccount

Rules and discipline:
- Rule
- RuleEvaluation
- Violation

Context and monitoring:
- ContextEvent
- ContextLink
- ThesisAssessment
- RegimeAssessment

Journaling and review:
- JournalEntry
- TradeReview

Revision and lifecycle:
- RevisionLog
- LifecycleEvent

Reconciliation and external linkage:
- ExternalMapping
- ReconciliationRun
- ReconciliationIssue

Important modeling rule:
- Do not collapse idea, thesis, plan, position, order, fill, and review into one object.
- They are separate concepts with separate timing and ownership.

Documentation structure:
- ADRs live in root/doc/adr/
- design and other docs live in root/doc/
- all docs use YAML front matter followed by Obsidian-friendly markdown body
- all diagrams should be ASCII only

ADRs already established:
- ADR-001: Modular Monolith with Layered Intelligence Architecture
- ADR-002: Separation of Deterministic Rules and Contextual Intelligence
- ADR-003: Hybrid Development Strategy with Docker for Infrastructure and Optional Full Containerization Later
- ADR-004: Canonical Domain Model and Source-of-Truth Boundaries

Other established docs:
- README.md
- system-blueprint.md
- domain-model.md

Development workflow:
- use GitHub Desktop
- issue-based development only
- no code without an explicit issue
- work is milestone/phase based
- project name remains trading-system for now

Current ask:
I want to continue from this exact foundation and move into the next design/build step without rehashing basics. Please work from this context and be practical, opinionated, and grounded.

