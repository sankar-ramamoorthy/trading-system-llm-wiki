
• Proposed Plan


  # Milestone 6C Issue 2: Massive Daily Bars Adapter

  ## Summary

  Implement a narrow Massive.com daily OHLCV adapter behind the existing MarketDataProviderRegistry, preserving
  yfinance as the default and keeping provider data read-only, advisory, and stored as MarketContextSnapshot.

  Sources verified: Massive official client uses RESTClient and list_aggs(...); latest GitHub release shown as v2.6.0
  on April 28, 2026.

  ## Key Changes

  - Add massive>=2.6,<3.0 to pyproject.toml and update uv.lock.
  - Add trading_system.infrastructure.massive.market_data_source.MassiveDailyOHLCVImportSource.
  - Register provider name massive in MarketDataProviderRegistry.create_daily_ohlcv_source(...).
  - Keep CLI shape unchanged:

    uv run trading-system fetch-market-data AAPL --provider massive --start 2026-04-01 --end 2026-04-30

  ## Adapter Behavior

  - Read API key from MASSIVE_API_KEY; fail with a clear ValueError if missing or blank.
  - Import massive.RESTClient lazily so tests can run without live network calls.
  - Call:

    client.list_aggs(
        ticker=symbol,
        multiplier=1,
        timespan="day",
        from_=start.isoformat(),
        to=end.isoformat(),
        adjusted=False,
        sort="asc",
        limit=50000,
    )
  - Normalize returned aggregate objects into the existing daily_ohlcv payload shape:
    symbol, provider: "massive", interval: "1d", start, end, adjusted: False, bars.
  - Each bar should include date, open, high, low, close, volume, plus optional provider fields only if present and
    useful: vwap, transactions, otc.
  - Derive date from Massive aggregate timestamp milliseconds in UTC.
  - Set observed_at to midnight UTC for the latest returned bar date.
  - Set source metadata to include symbol, date range, provider, interval/timespan, and adjustment:
    symbol=AAPL;start=2026-04-01;end=2026-04-30;provider=massive;timespan=day;adjusted=false.

  ## Failure Rules

  - Missing dependency: ValueError("massive is not installed.").
  - Missing key: ValueError("MASSIVE_API_KEY is required for Massive.com market data.").
  - Provider exceptions: wrap as ValueError("Massive.com daily bars fetch failed for SYMBOL: ...").
  - Empty results: ValueError("No daily OHLCV data returned for symbol SYMBOL.").
  - Invalid bars: fail clearly for missing timestamp, missing OHLCV values, invalid numeric values, or non-integer
    volume.
  - No fallback between Massive and yfinance.

  ## Tests

  - Add tests/test_massive_market_data_source.py covering:
      - successful payload normalization from fake aggregate objects
      - missing MASSIVE_API_KEY
      - empty provider result
      - invalid date range
      - provider exception wrapping
      - invalid/missing aggregate fields
  - Update tests/test_market_data_provider_registry.py:
      - massive resolves successfully when MASSIVE_API_KEY is set
      - unsupported provider test uses a truly unsupported name
      - yfinance behavior remains unchanged
  - Update tests/test_cli_market_data_fetch.py:
      - missing API key fails without creating a snapshot
    uv run pytest tests\test_massive_market_data_source.py tests\test_market_data_provider_registry.py
tests\test_cli_market_data_fetch.py tests\test_yfinance_market_data_source.py
  - Then run full suite:

    uv run pytest

  ## Docs And Status

  - Update README.md, STATUS.md, DOCS/product-roadmap.md, and DOCS/milestone-6-market-data-provider-design.md to say
    Massive daily bars are implemented.
  - Add a Milestone 6C Issue 2 closeout doc mirroring prior closeout style.
  - Keep ADR-009 as-is unless implementation discovers a material contradiction.

  ## Assumptions

  - Use the official massive client package, not custom HTTP.
  - yfinance remains the default provider.
  - Tests must use fake clients and must not make live network calls.
  - Sources used: https://github.com/massive-com/client-python and
    https://raw.githubusercontent.com/massive-com/client-python/master/massive/rest/aggs.py.

