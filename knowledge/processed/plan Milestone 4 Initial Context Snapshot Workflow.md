  # Milestone 4 Initial Context Snapshot Workflow

  ## Summary

  Implement the first Milestone 4 slice as a read-only local file import workflow for market/context snapshots.
  Context snapshots are stored locally, timestamped, auditable, and visibly separate from canonical trade records.

  The first workflow attaches each snapshot to an instrument_id and may optionally link it to a TradePlan, Position,
  or TradeReview. External context informs planning and review but never changes domain meaning, rule results,
  approvals, fills, or position state.

  ## Key Changes

  - Add a new domain record, MarketContextSnapshot, with:
      - id: UUID
      - instrument_id: UUID
      - optional target_type: TradePlan, Position, or TradeReview
      - optional target_id: UUID
      - context_type: str, for example price_snapshot, calendar, annotation, or reference
      - source: str
      - source_ref: str | None, usually the imported file path or external reference
      - observed_at: datetime
      - captured_at: datetime
      - payload: dict[str, object]
  - Add ports:
      - MarketContextSnapshotRepository
          - add(snapshot)
          - get(snapshot_id)
          - list_by_instrument_id(instrument_id)
          - list_by_target(target_type, target_id)
      - MarketContextImportSource
          - loads a local JSON context document and returns validated snapshot input data
  - Add infrastructure:
      - JSON persistence collection: market_context_snapshots
      - in-memory repository for tests
      - local JSON-file import adapter that reads one context document from disk
      - no network provider, credentials, live feed, streaming, or background refresh
  - Add services:
      - MarketContextImportService
          - validates target existence when target_type/target_id is provided
          - derives instrument_id from TradePlan, Position, or TradeReview when possible
          - otherwise requires explicit instrument_id
          - stores a new immutable snapshot
      - MarketContextQueryService
          - lists snapshots by instrument or linked target
          - returns snapshot detail by ID
  - Add CLI commands:
      - import-context <file> [--instrument-id UUID] [--target-type trade-plan|position|trade-review] [--target-id
        UUID] [--source NAME]
      - list-context --instrument-id UUID
      - list-context --target-type trade-plan|position|trade-review --target-id UUID
      - show-context <snapshot-id>
  - Accepted import file shape:

    {
      "context_type": "price_snapshot",
      "observed_at": "2026-04-26T16:00:00-04:00",
      "payload": {
        "symbol": "AAPL",
        "last": "185.25",
        "notes": "Delayed close snapshot"
      }
    }

  ## Test Plan
      - import stores an immutable snapshot with captured_at
      - JSON repository round-trips snapshots after reload
      - snapshots list by instrument and by target
      - invalid/corrupt context import JSON reports a clear error
  - CLI tests:
      - import-context prints market_context_snapshot_id
      - list-context shows stable table output and empty state

  ## Assumptions

  - First source is file import, not live market data or a stub provider.
  - Context snapshots are instrument-based and may optionally link to a plan, position, or review target.
  - Imported payload stays flexible JSON for this slice, but required envelope fields are validated.
  - Context remains read-only support data and is not added to deterministic rule evaluation.
  - No ADR is required for this first slice unless the implementation expands into provider selection, retention
    policy, or external API integration.



› i need to understand this a bit more. There’s one product decision that will materially change the first
  implementation: whether Milestone 4 starts with
    manually entered/local snapshot context or tries to pull from an external data source. Given the repo’s current
    dependency set and no-network/local-first posture, I recommend a local snapshot source for the first slice.

  • Questions 2/2 answered
    • What should the first Milestone 4 context source be?  << especially this. what are the pros and cons. per our
  plan will we be ok if we use a dummy fallback provider such as yafinance?  will we need a ADR?


• Searching the web

• Searched yfinance PyPI package disclaimer Yahoo Finance personal use only

────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

• Yes, you can use yfinance as a dummy/fallback provider, but I would not make it the first implementation path. I’d
  design the boundary so yfinance can plug in cleanly after the local/file workflow exists.

  The decision is really this:

  Option A: Local/file snapshot first
  Pros:

  - Fits the current project direction: local-first, explicit, auditable, no automation.
  - No credentials, network failures, rate limits, dependency churn, provider terms, or live-data ambiguity.
  - Lets us design the core model cleanly: “a context snapshot was captured at time X from source Y.”
  - Easier tests: deterministic JSON in, deterministic stored snapshot out.

  Cons:

  - You don’t get real market data automatically.
  - You have to prepare or export the data yourself.
  - It may feel less useful day-to-day until a provider is added.

  Option B: yfinance provider first
  Pros:

  - Immediately useful for common quote/history snapshots.
  - Good enough for personal research and prototypes.
  - Lets the CLI do something real like fetch-context --symbol AAPL.

  Cons:

  - Adds external dependency and network behavior to a repo that currently has no market-data dependency.
  - yfinance is unofficial. Its PyPI page says it is not affiliated with Yahoo and is intended for research/educational/
    personal use, so it should not become a durable source-of-truth foundation.
  - It introduces failure cases: missing symbols, changed upstream responses, delayed/partial data, request failures,
    throttling, timezone/calendar quirks.
  - It tempts the project toward “live market data” unless we strictly snapshot the result and never stream.

  My recommendation: use a provider interface now, implement file import first, then add yfinance as an optional adapter
  behind that interface.

  That gives you the right architecture:

  CLI command
    -> Context service
      -> Context source port
        -> File import adapter now

  For yfinance, the rule should be:

  yfinance is an optional read-only snapshot source, not canonical market infrastructure.

  It can fetch a quote or price history, then immediately store the result as a timestamped MarketContextSnapshot. The
  rest of the app only sees the stored snapshot, not yfinance directly.

  On the ADR question: not for file import only. The accepted Milestone 4 docs already cover read-only context snapshots
  well enough.

  But I would create a small ADR if we add yfinance or any external provider. The ADR does not need to be huge. It
  should record:

  - external providers are read-only snapshot sources
  - provider data is non-canonical and advisory
  - no streaming, no execution triggers, no automated plan creation
  - fetched data must be stored as an auditable snapshot
  - yfinance is optional/prototype-grade, not a guaranteed production data provider
  - provider failures must not block core trade workflow

  So the revised plan I’d use is: design provider boundary now, implement file import first, allow yfinance as a later
  optional adapter with an ADR when introduced.


› ok please go ahead and implement and document


• I can’t mutate files while this session is still in Plan Mode, so here is the implementation-ready plan for the next
  execution turn.

• Proposed Plan


  # Implement Milestone 4 Context Snapshot Slice

  ## Summary

  Add the first read-only market context workflow using local JSON file import as the initial source, with a provider/
  source boundary that can later support yfinance or another external provider. Store imported context as timestamped,
  auditable snapshots linked to an instrument and optionally to a TradePlan, Position, or TradeReview.

  Document that external providers are deferred and require an ADR when introduced.

  ## Implementation Changes

  - Add MarketContextSnapshot as a domain record under trading context:
      - UUID identity
      - instrument_id
      - optional target_type and target_id
      - context_type, source, optional source_ref
      - observed_at, captured_at
      - flexible JSON payload
      - no behavior that mutates trade plans, positions, reviews, rules, or lifecycle state
  - Add ports:
      - MarketContextSnapshotRepository with add, get, list_by_instrument_id, and list_by_target
      - MarketContextImportSource for reading a local JSON context document into validated snapshot input
  - Add infrastructure:
      - JSON persistence collection market_context_snapshots
      - JSON repository serialization/deserialization
      - in-memory repository for tests
      - file import adapter for this JSON shape:

        {
          "context_type": "price_snapshot",
          "observed_at": "2026-04-26T16:00:00-04:00",
          "payload": {
            "symbol": "AAPL",
            "last": "185.25",
            "notes": "Delayed close snapshot"
          }
        }
  - Add services:
      - MarketContextImportService
          - imports from the source adapter
          - validates optional target existence
          - derives instrument_id from TradePlan, Position, or TradeReview when linked to a target
          - requires explicit instrument_id when no target is supplied
          - stores the immutable snapshot
      - MarketContextQueryService
          - retrieves snapshot detail
          - lists snapshots by instrument or by target
  - Add CLI commands:
      - import-context <file> [--instrument-id UUID] [--target-type trade-plan|position|trade-review] [--target-id
        UUID] [--source NAME]
      - list-context --instrument-id UUID
      - list-context --target-type trade-plan|position|trade-review --target-id UUID
      - show-context <snapshot-id>

  ## Documentation

  - Update STATUS.md and PROJECT.md to show Milestone 4 has begun with local context snapshot import.
  - Update README.md with the new CLI workflow and example import file.
  - Add or update a Milestone 4 note in DOCS/ explaining:
      - file import is the first source
      - context snapshots are read-only and non-canonical
      - provider-based fetches are intentionally deferred
      - yfinance may be added later only behind the source port
  - Do not add an ADR for file import alone.
  - Add an ADR only when introducing yfinance or another external data provider, recording:
      - provider data is advisory and non-canonical
      - all provider output is stored as explicit snapshots
      - no streaming, execution triggers, broker coupling, or automated plan creation
      - provider failure must not block core trading workflow

  - Add service tests for:
      - target validation failures
      - instrument derivation from linked targets
      - no mutation of canonical trade records
  - Add persistence tests for:
      - JSON round-trip after repository reload
      - list by instrument
      - list by target
      - invalid import JSON errors
  - Add CLI tests for:
      - successful import-context
      - list-context empty and populated states
      - show-context detail output
      - invalid UUID and missing target errors

  ## Assumptions

  - The first implementation uses local JSON file import, not yfinance.
  - The code should include the source/provider port now so yfinance can be added later without changing service or
    repository shape.
  - yfinance is acceptable later as a prototype-grade optional provider, but adding it should include an ADR.

