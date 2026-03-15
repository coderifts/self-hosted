# Self-Hosted CodeRifts — Setup Guide

Run CodeRifts on your own infrastructure using Docker and Docker Compose.

## Prerequisites

- **Docker Desktop** or **Docker Engine** + **Docker Compose** v2

## Quick Start

```bash
# Clone the repository
git clone https://github.com/coderifts/app.git
cd app

# Copy the environment template and fill in your values
cp .env.example .env
# Edit .env with your GitHub App credentials

# Start all services
docker compose up -d
```

## Health Check

```bash
curl http://localhost:3000/health
# Expected: {"status":"ok"}
```

## API Test

```bash
curl -X POST http://localhost:3000/api/v1/diff \
  -H "Content-Type: application/json" \
  -d '{
    "api_key": "YOUR_API_KEY",
    "before": "openapi: 3.0.0\ninfo:\n  title: API\n  version: 1.0.0\npaths:\n  /users:\n    get:\n      summary: List\n      responses:\n        \"200\":\n          description: OK",
    "after": "openapi: 3.0.0\ninfo:\n  title: API\n  version: 2.0.0\npaths: {}"
  }'
# Expected: JSON response with breaking change analysis
```

## Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `GITHUB_APP_ID` | Yes | Your GitHub App's numeric ID |
| `GITHUB_PRIVATE_KEY` | Yes | Your GitHub App's private key (PEM format) |
| `GITHUB_WEBHOOK_SECRET` | Yes | The webhook secret configured in your GitHub App |
| `NODE_ENV` | No | Set to `production` for production deployments |

## Data Persistence

PostgreSQL and Redis data are stored in Docker volumes (`postgres_data` and `redis_data`). These persist across container restarts.

To reset all data:

```bash
docker compose down -v
docker compose up -d
```

## Updating

```bash
docker compose pull
docker compose up -d
```
