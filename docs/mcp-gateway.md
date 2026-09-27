# API.market MCP Gateway guide

## Connection URL

Use this exact URL when adding the hosted API.market MCP Gateway to an AI client:

```text
https://api.market/api/mcp/gateway
```

The transport is **Streamable HTTP**. The gateway requires an API.market account and authentication through OAuth or an API key.

- `https://api.market/api/mcp/gateway` is the MCP connection endpoint.
- `https://api.market/mcp` is the human-readable setup page, not an MCP endpoint.
- `https://api.market` is the website.

Older documentation may show a prod hostname or product-specific `/api/mcp/{workspace}/{api-slug}` URLs. Those examples describe a different or legacy connection path. For new gateway connections, use the exact gateway URL above. Do not substitute a product-specific path in a gateway configuration. Backend API execution hosts and compatibility settings are separate from the URL a client should use to connect.

## Connect with OAuth

Choose a client that supports remote Streamable HTTP MCP servers and OAuth. Add the gateway URL, then complete sign-in and consent with your API.market account. OAuth discovery, dynamic client registration and PKCE are supported.

### Claude Code

```sh
claude mcp add api-market --transport http https://api.market/api/mcp/gateway
```

Open `/mcp` in Claude Code and authenticate the `api-market` server when prompted.

### Cursor manual configuration

```json
{
  "mcpServers": {
    "api-market": {
      "url": "https://api.market/api/mcp/gateway"
    }
  }
}
```

Complete the client's OAuth flow. Client settings and feature availability can vary by version; see the [live setup page](https://api.market/mcp) for additional instructions.

## API-key alternative

Use an API.market API key in the `x-api-market-key` header. Keep the key in your local client configuration or its supported secret store; never commit it to a repository or paste it into public support requests.

For clients accepting `url` and `headers` under `mcpServers`:

```json
{
  "mcpServers": {
    "api-market": {
      "url": "https://api.market/api/mcp/gateway",
      "headers": {
        "x-api-market-key": "YOUR_API_MARKET_KEY"
      }
    }
  }
}
```

Replace the placeholder locally. OAuth does not require this API-key header.

## Gateway workflow

The gateway exposes five tools. Discover their current schemas through your client's MCP tool listing instead of assuming the fields used by a product-specific server are interchangeable.

| Tool | Purpose |
| --- | --- |
| `search_apis` | Find API products by keyword or category. |
| `get_api_tools` | Read an API's operation schemas, documentation and pricing. |
| `call_api` | Execute a selected API operation. |
| `check_usage` | Inspect usage, quota and wallet balance. |
| `manage_subscription` | Inspect plans and manage API subscriptions. |

Start by finding the product and inspecting its tools and pricing. Execute only the operation the user has requested. A gateway connection does not automatically subscribe an account to every API, and thousands of catalog operations are not thousands of immediately loaded gateway tools.

## Pricing and permissions

Free tiers, subscriptions and usage charges vary by API. Execution can consume quota or wallet funds; subscription changes can create paid commitments. Confirm the price and the user's authorization before paid actions. Use the subscription tool's dry-run option before a paid subscription change and present its cost for confirmation.

Requests go to API.market and the selected provider as required to perform the requested operation. Review the [terms](https://api.market/terms_of_service), [privacy policy](https://api.market/privacy_policy) and provider documentation before sending sensitive data.

The hosted gateway implementation is proprietary. This repository contains public client integration metadata and documentation, not the hosted server's source code.

## Connection troubleshooting

**An unauthenticated request returns HTTP 401.** This is the expected authentication challenge, not proof that the endpoint is broken. Sign in through OAuth or configure the API-key header. A successful unauthenticated metadata request alone does not establish that authenticated tool discovery or API execution works.

**The client receives HTML.** Check that it is using `/api/mcp/gateway`, not the `/mcp` landing page or the website root.

**A guide or directory shows a different hostname.** Use the connection URL at the top of this guide. The [official registry record](https://registry.modelcontextprotocol.io/v0.1/servers/io.github.Noveum%2Fapi-market-gateway/versions/3.0.0) and [integration manifest](../server.json) identify the same Streamable HTTP endpoint.

**A tool reports a subscription, quota or balance problem.** Inspect the applicable API plan and account usage. Reconnecting to MCP does not change a provider's pricing or your account entitlements.

## Verification scope

On September 24, 2026, the gateway passed an authenticated protocol audit covering OAuth authorization-code exchange with S256 PKCE, initialization, tool discovery, resources and prompts. It reported version 3.0.0, five tools, two resources and two prompts. Claude Code 2.1.281 also completed OAuth and discovered the five tools. No paid execution or subscription changes were performed.

On September 27, 2026, an unauthenticated request to the canonical gateway returned HTTP 401 and pointed to [protected-resource metadata](https://api.market/.well-known/oauth-protected-resource/api/mcp/gateway). That metadata returned HTTP 200 and identified `https://api.market/api/mcp/gateway` as its resource, with `https://api.market` as the authorization server.

These checks do not certify every client version or every underlying API operation. See the [repository README](../README.md) for client-specific validation limits.
