# ACCESS Argilla

Argilla instance for the ACCESS QA system - human review of Q&A pairs before they enter the RAG database.

## Local Development

### Prerequisites

- Docker and Docker Compose

### Setup

1. Copy the example environment file:
```bash
   cp .env.example .env
```

2. Start the services:
```bash
   docker compose up -d
```

3. Open http://localhost:6900

### Default Credentials (local dev)

- Username: `argilla`
- Password: `12345678`
- API Key: `argilla.apikey`

### Stopping
```bash
docker compose down
```

### Troubleshooting

**Elasticsearch crashes with exit code 137**

This is an out-of-memory error. Either:
- Stop other Docker containers to free memory
- Increase Docker's memory allocation in Docker Desktop settings

## Services

| Service | Port | Purpose |
|---------|------|---------|
| Argilla | 6900 | Web UI + API |
| Elasticsearch | 9200 | Search/vector store |
| PostgreSQL | 5432 | Database |
| Redis | 6379 | Caching/queues |

## Deployment

TODO: GitHub Actions workflow for production deployment