# API.market agent integrations

Connect AI agents to the [API.market hosted MCP Gateway](https://api.market/mcp). This repository contains client integration metadata and setup documentation, not the implementation of the hosted service.

## Cursor plugin

The `api-market` plugin in `plugins/api-market` configures this Streamable HTTP endpoint:

```text
https://api.market/api/mcp/gateway
```

Sign in through the client's OAuth prompt with your API.market account. OAuth supports dynamic client registration and PKCE. Do not put API keys or access tokens into this repository.

The plugin is prepared for marketplace review; publication in the Cursor marketplace is not implied. Its package version is independent of the hosted server version.

### Manual Cursor setup

You can connect without waiting for marketplace review. Add this configuration in Cursor's MCP settings:

```json
{
  "mcpServers": {
    "api-market": {
      "url": "https://api.market/api/mcp/gateway"
    }
  }
}
```

Complete OAuth sign-in when prompted. See the [live setup instructions](https://api.market/mcp) for API-key authentication and other supported clients.

## Claude Code plugin

Add this repository as a plugin marketplace in Claude Code:

```text
/plugin marketplace add Noveum/api-market-agent-integrations
/plugin install api-market@api-market
```

The plugin configures the hosted Streamable HTTP gateway at `https://api.market/api/mcp/gateway`. Open `/mcp` to authenticate with your API.market account through OAuth. This is an API.market-maintained integration; inclusion in an official client marketplace is not implied.

The Claude Code plugin and marketplace manifests are validated with `claude plugin validate`. On September 24, 2026, Claude Code v2.1.281 loaded the plugin via `--plugin-dir`, completed OAuth in `/mcp`, and connected with all five gateway tools plus resource and prompt capabilities. No model-driven API execution or paid operations were performed during this client connection test. The independent live OAuth and MCP protocol audit is described below.

## Gemini CLI extension

Install the extension from this repository:

```sh
gemini extensions install https://github.com/Noveum/api-market-agent-integrations
```

The root `gemini-extension.json` configures `https://api.market/api/mcp/gateway` using Gemini CLI’s `httpUrl` Streamable HTTP transport. In an interactive Gemini session, use `/mcp auth api-market` to complete OAuth with your API.market account. Never put credentials in the manifest.

Manifest validation and isolated local extension loading are tested with Gemini CLI 0.26.0. A full authenticated Gemini session and API execution have not been tested; the separate live protocol audit below does not replace a client-specific test. Gallery indexing is automatic after repository tagging and remains subject to the gallery’s validation.

## What agents can do

The gateway advertises a catalog of 580+ APIs for image and video generation, search, scraping, maps, data and other workflows. Five gateway tools expose discovery and execution:

| Tool | Purpose |
| --- | --- |
| `search_apis` | Find products by keyword or browse categories |
| `get_api_tools` | Retrieve operation schemas, documentation and pricing |
| `call_api` | Execute a selected API operation |
| `check_usage` | Check subscription usage, quotas and wallet balance |
| `manage_subscription` | Inspect plans and manage API subscriptions |

Catalog operations are discovered through these tools; the gateway does not load thousands of tool schemas into the client at once.

## Accounts, permissions and pricing

An API.market account is required. Individual APIs have different free tiers, subscriptions and usage charges. OAuth does not remove those requirements. API execution can consume quota or wallet funds, and subscription management can change paid subscriptions. Inspect prices and ask the user before paid execution or paid subscription changes. For paid subscription changes, use the gateway's dry-run option first and present the cost before confirmation.

Requests are sent to API.market and selected API providers as needed to perform the requested operation. Review the [API.market Terms of Service](https://api.market/terms_of_service), [Privacy Policy](https://api.market/privacy_policy), and each API's documentation before sending sensitive data.

## Validation

On September 24, 2026, a protocol audit completed dynamic registration, OAuth authorization-code exchange with PKCE S256, authenticated initialization, `tools/list`, `resources/list` and `prompts/list`. The hosted server reported version 3.0.0, five tools, two resources and two prompts. No paid API operations or subscription changes were executed. The temporary audit token was revoked afterward.

This validates the hosted protocol flow, not every client or every underlying API operation. The Cursor plugin configuration follows the official template; it has not been tested through Cursor's installed plugin UI.

## Support

Setup: https://api.market/mcp

Submission contact: shashank@noveum.ai
