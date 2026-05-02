
● Milestone 7G is complete. Here's the summary:

  What was validated:
  - docker compose up --build — both containers healthy
  - Parse → qwen/qwen3-32b via Groq correctly extracts fields and surfaces validation issues
  - Save → creates linked TradeIdea, TradeThesis, TradePlan records
  - Persist → confirmed in store.json with approval_state: draft
  - All error paths: empty input, missing fields, ambiguous fields, unknown symbol, unknown plan ID
  - uv run pytest: 216 passed

  Infrastructure fixes made during acceptance:
  - Switched LLM from Ollama llama3.1 (unavailable) → Groq qwen/qwen3-32b (60 RPM free tier)
  - Wired env_file into docker-compose so GROQ_API_KEY reaches the container
  - Hardened the LiteLLM parser to tolerate small-model output variance (string-as-list, non-string candidates)

  Next: Milestone 7H — Milestone Closeout (tests, docs, roadmap updates, KB synthesis).