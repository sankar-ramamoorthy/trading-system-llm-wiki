• Proposed Plan


  # Issue 7A: Dockerized Runtime Skeleton

  ## Summary

  After leaving Plan Mode, first document the full Milestone 7 issue map clearly, then implement Issue 7A as the
  runtime foundation.

  7A does not implement trade capture yet. It creates the local containerized app shell: backend API container,
  frontend web container, shared env/config, host Ollama connectivity, and startup docs. Ollama itself should not run
  in Compose because it already runs on the host.

  ## Key Changes

  - Document the Milestone 7 issue breakdown:
      - 7A runtime foundation
      - 7B reference lookup
      - 7C draft contract
      - 7D natural-language parser boundary
      - 7E FastAPI trade-capture service
      - 7F React/Vite workspace
      - 7G end-to-end save workflow
      - 7H closeout
  - Add Docker Compose for two app services:
      - api: Python backend using ghcr.io/astral-sh/uv:python3.12-bookworm-slim
      - web: Vite React TypeScript frontend
  - Add minimal FastAPI backend skeleton:
      - GET /health
      - no trade-capture endpoints yet
      - no domain workflow changes yet
  - Add minimal Vite React TypeScript frontend:
      - basic app shell
      - backend health check display
      - no trade-capture UI yet
  - Configure host Ollama access for future 7D work:
      - default backend env points to http://host.docker.internal:11434
      - include placeholders for LiteLLM model/provider settings
      - do not add an Ollama container in 7A
      - backend continues to use existing trading_system package layout
  ## Test Plan

  - Backend unit/API test:
      - FastAPI health endpoint returns success.
  - Compose/manual validation:
      - docker compose up starts api and web.
      - API health endpoint is reachable from host.
      - Web app is reachable from host.
      - Web app can call backend health endpoint.
  - Regression:
      - existing uv run pytest still passes.
      - existing CLI entrypoint still works.

  ## Assumptions

  - 7A uses Vite React TypeScript because this is the preferred learning path and durable frontend baseline.
  - Ollama runs on the host, not in Docker.
  - LiteLLM/OpenAI/Groq model routing is planned for 7D, but 7A may add env placeholders only.
  - No parser, lookup, draft schema, or save workflow is implemented in 7A.
  - No Postgres, broker integration, auth, cloud deployment, or production hardening is introduced.