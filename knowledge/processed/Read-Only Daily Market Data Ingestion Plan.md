 Proposed Plan


  # Read-Only Daily Market Data Ingestion Plan

  ## Summary

  Implement the first Milestone 6 provider slice: an explicit CLI command that fetches daily OHLCV history from yfinance and stores it as a non-canonical
  MarketContextSnapshot.

  The command will be fetch-market-data <symbol> --start YYYY-MM-DD --end YYYY-MM-DD, with the same optional --instrument-id, --target-type, and --target-
  id linking behavior as import-context. The first implementation stores raw OHLCV plus adjusted close, uses yfinance as a main dependency, and does not
  add live quotes, streaming, scheduled refresh, recommendations, or broker behavior.

  External references used for this plan:

  - yfinance PyPI shows latest 1.3.0 released April 16, 2026 and repeats the unofficial/personal-use caveat: https://pypi.org/project/yfinance/
  - yfinance download docs support start, end, interval='1d', and auto_adjust: https://ranaroussi.github.io/yfinance/reference/api/yfinance.download.html

  ## Key Changes

  - Add yfinance>=1.3,<2.0 to main project dependencies.
  - Add a yfinance context-source adapter, likely under src/trading_system/infrastructure/yfinance/, that implements the existing
    MarketContextImportSource protocol.
  - Adapter behavior:
      - call yfinance.download(symbol, start=start, end=end, interval="1d", auto_adjust=False, actions=False, progress=False, threads=False,
        multi_level_index=False)
      - reject missing/blank symbols, invalid date order, empty results, missing required columns, null OHLCV values, and provider exceptions with clear
        ValueError messages
      - produce ImportedMarketContext(context_type="daily_ohlcv", observed_at=<last returned bar date at 00:00 UTC>, payload=...)
      - payload shape:
          - symbol
          - provider: "yfinance"
          - interval: "1d"
          - start
          - end_exclusive
          - auto_adjust: false
          - bars: list of objects with date, open, high, low, close, adj_close, volume
  - Add CLI command:
      - fetch-market-data <symbol> --start YYYY-MM-DD --end YYYY-MM-DD [--instrument-id UUID] [--target-type trade-plan|position|trade-review --target-id
        UUID]
      - source is fixed to yfinance
      - source_ref is a stable string containing symbol, start, end, interval, and auto_adjust=false
      - if no target is supplied, --instrument-id is required
  - Update docs:
      - knowledge base sync after app repo implementation is complete

  ## Test Plan

  - Add focused adapter tests with monkeypatched/fake yfinance download function:
      - valid dataframe converts to daily_ohlcv payload with raw OHLCV plus adj_close
      - start date must be before end date
      - empty provider result fails without creating a snapshot
      - missing required columns fails clearly
      - null OHLCV/volume values fail clearly
      - provider exception is wrapped as a clear ValueError
  - Add service/CLI tests:
      - fetch-market-data AAPL --start 2026-04-01 --end 2026-04-05 --instrument-id <uuid> stores a snapshot
      - target linking works for trade-plan, and the fetched snapshot appears in show-trade-plan metadata
      - missing --instrument-id without target fails with existing instrument-required message
      - target/instrument mismatch still fails through existing service validation
      - show-context displays the stored payload and list-context --context-type daily_ohlcv --source yfinance finds it
  - Run:
      - uv run pytest tests\test_market_context_service.py tests\test_cli_market_context.py
      - new provider-focused test file, if separate
      - uv run pytest

  ## Assumptions

  - The yfinance adapter is prototype-grade and network-backed, but tests must not hit the network.
  - The command fetches one symbol per invocation.
  - --end follows yfinance semantics: exclusive end date.
  - Stored numeric values should be JSON-safe numbers; volume should be an integer.
  - No schema migration is needed because MarketContextSnapshot.payload is already flexible JSON.
  - No new domain entity is introduced; provider output stays behind the context snapshot boundary.