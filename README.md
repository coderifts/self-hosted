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

## MCP Server

CodeRifts is available as a Model Context Protocol (MCP) server so AI agents can run governance and contract-safety checks before they call or merge API changes. The server speaks MCP JSON-RPC over Streamable HTTP.

- **Registry name:** `io.github.coderifts/api-governance`
- **Transport:** Streamable HTTP
- **Hosted endpoint:** `https://app.coderifts.com/mcp`
- **Self-hosted endpoint:** `https://<your-host>/mcp` (same path on your own instance)
- **Authentication:** send `Authorization: Bearer cr_live_YOUR_KEY` on tool calls. The `initialize` and `tools/list` methods are open (no key required) so clients can discover the tools without authenticating.

### Tools

| Tool | Description |
| --- | --- |
| `preflight_check` | Analyze an API spec diff before merge. Returns risk score, blast radius, agent impact, and a merge decision (ALLOW / WARN / REQUIRE_APPROVAL / BLOCK). |
| `agent_tool_check` | Check whether an API change breaks AI agent tool calling (endpoint removal, newly required fields, result-shape drift). |
| `agent_readiness_score` | Score an OpenAPI spec or MCP manifest for AI-agent readiness (0–100) with recommendations. |
| `registry_validate` | Validate an MCP tool registry or OpenAPI spec collection for governance health. |
| `agent_preflight` | Pre-flight governance check for agent workflows, given tool schemas before and after a change. |
| `traffic_analyze` | Infer API spec drift from HTTP traffic samples, without needing spec files. |
| `mcp_diff` | Compare two MCP manifests and detect breaking changes in tool schemas, input/output types, and descriptions. |
| `governance_health` | Governance health score for an API spec (A–F grade, policy compliance, recommendations). |

### Claude Desktop

Add CodeRifts to `claude_desktop_config.json` using the remote bridge:

```json
{
  "mcpServers": {
    "coderifts": {
      "command": "npx",
      "args": [
        "mcp-remote",
        "https://app.coderifts.com/mcp",
        "--header",
        "Authorization: Bearer cr_live_YOUR_KEY"
      ]
    }
  }
}
```

Replace `cr_live_YOUR_KEY` with your CodeRifts API key, restart Claude Desktop, and the eight tools above become available.

### Quick check

```bash
# List tools (no key required)
curl -s -X POST https://app.coderifts.com/mcp \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

## Documentation

For full documentation, visit [coderifts.com/docs](https://coderifts.com/docs).

## License

See [coderifts.com](https://coderifts.com) for licensing terms.
