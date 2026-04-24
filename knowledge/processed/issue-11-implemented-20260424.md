Implemented Issue 11’s narrow OrderIntent slice.

  The change adds a first-class src/trading_system/domain/trading/order_intent.py entity plus repository support, a gated src/trading_system/services/
  create_order_intent_service.py, and an optional order_intent_id on src/trading_system/domain/trading/fill.py. Creation now requires an existing approved
  TradePlan and persisted passing RuleEvaluations, while FillService validates that any linked order intent belongs to the same plan as the position.
  Position opening timing was left unchanged.

  I also extended the in-memory and JSON repositories, the read model in src/trading_system/services/position_query_service.py, and the CLI in src/
  trading_system/app/cli.py. show-position now displays order intents before fills, and demo-planned-trade now creates an order intent before recording
  linked manual fills.

  Verification: uv run pytest -> 53 passed in 3.11s.