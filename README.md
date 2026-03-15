# CodeRifts — Self-Hosted Deployment

Docker Compose configuration for self-hosting CodeRifts on your own infrastructure.

## Quick Start

```bash
# 1. Clone this repo
git clone https://github.com/coderifts/self-hosted.git
cd self-hosted

# 2. Copy and configure environment variables
cp .env.example .env
# Edit .env with your values

# 3. Start all services
docker compose up -d
```

## What's Included

- **docker-compose.yml** — Orchestrates the CodeRifts app, PostgreSQL database, and Redis cache
- **.env.example** — Template for required environment variables
- **SELF_HOSTED.md** — Detailed deployment and configuration guide

## Documentation

For full documentation, visit [coderifts.com/docs](https://coderifts.com/docs).

## License

See [coderifts.com](https://coderifts.com) for licensing terms.
