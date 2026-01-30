# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Argilla instance for the ACCESS QA system — human review of Q&A pairs before they enter the RAG database. This is a Docker Compose deployment project (infrastructure-as-code), not a custom application codebase. There is no build step, test suite, or linter.

## Commands

```bash
cp .env.example .env        # First-time setup: create local env file
docker compose up -d         # Start all services
docker compose down          # Stop all services
docker compose logs -f       # Tail logs from all services
```

Access the Argilla UI at http://localhost:6900 (default creds: argilla / 12345678 / API key: argilla.apikey).

## Architecture

Five Docker Compose services on a shared bridge network (`argilla`):

- **argilla** (port 6900) — Argilla web UI and REST API server
- **worker** — Background job processor (same image as argilla, runs `argilla_server worker` with 2 workers)
- **postgres** (port 5432) — PostgreSQL 14, stores metadata and config (database: `argilla`)
- **elasticsearch** (port 9200) — Elasticsearch 8.17.0, single-node, handles search/vector indexing (512MB heap)
- **redis** (port 6379) — Caching and background job queues

Data is persisted across restarts via named volumes: `argilladata`, `postgresdata`, `elasticdata`.

The `docker-compose.yaml` uses YAML anchors (`x-common-variables`) to share database/Redis/Elasticsearch connection strings between the argilla and worker services.

## Environment Variables

Defined in `.env` (from `.env.example`). Key variables: `ARGILLA_USERNAME`, `ARGILLA_PASSWORD`, `ARGILLA_API_KEY`, `POSTGRES_USER`, `POSTGRES_PASSWORD`.

## Troubleshooting

Elasticsearch exit code 137 = OOM. Free Docker memory or increase Docker Desktop memory allocation.
