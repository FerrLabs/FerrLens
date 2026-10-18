# FerrLens MCP

FerrLens as Model Context Protocol server, so an AI assistant can drive it.

[![npm @ferrlens/mcp](https://img.shields.io/npm/v/@ferrlens/mcp)](https://www.npmjs.com/package/@ferrlens/mcp)

## Quick start

Add to your MCP client configuration (Claude Code, Cursor, etc.):

```json
{
  "mcpServers": {
    "ferrlens": { "command": "npx", "args": ["-y", "@ferrlens/mcp"] }
  }
}
```

The FerrLens tools are anonymous: no account, no token, no OAuth flow. The API rate limits each caller by IP.

## Tools

Tools marked **destructive** are irreversible or high-impact. MCP clients should gate them behind a confirmation.

### `@ferrlens/mcp`

All tools are read-only and anonymous. Each one calls a public FerrLens endpoint on the caller's behalf, so the target has to be reachable from the internet; the API refuses URLs that point at private addresses.

| Tool                                                                                                                     | Notes                                                       |
| ------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------- |
| `dns_lookup`, `dns_propagation`, `reverse_dns`                                                                           | DNS                                                         |
| `check_email_auth`, `check_blacklist`, `verify_email`                                                                    | SPF, DMARC and DKIM; DNS blocklists; address checks         |
| `get_http_headers`, `check_security_headers`, `get_page_meta`, `preview_open_graph`, `check_mixed_content`, `check_cors` | HTTP and page checks                                        |
| `search_certificates`                                                                                                    | TLS certificates from Certificate Transparency logs         |
| `check_seo`, `check_robots_txt`, `check_sitemap`, `check_links`                                                          | `check_seo` runs Lighthouse and `check_links` crawls a site |
| `get_share`                                                                                                              | reads a published result snapshot                           |

FerrFlow CLI-specific tools (`dry_run`, `validate_config`, `read_config`, `read_changelog`, `list_release_tags`, `record_event`) were removed in v4.0.0. They required either a local FerrFlow CLI install or HMAC signing the MCP doesn't do. Use the FerrFlow CLI directly or fetch docs via `fetch_docs("ferrflow", "docs/...")`.

## Configuration

| Variable               | Description                                                           | Default                    |
| ---------------------- | --------------------------------------------------------------------- | -------------------------- |
| `FERRLENS_API_URL`     | Base URL for the FerrLens API (`@ferrlens/mcp`).                      | `https://api.ferrlens.com` |
| `FERRLENS_API_VERSION` | Contract version sent as `x-ferrlens-api-version` by `@ferrlens/mcp`. | `2026-08-04`               |

Shared variables (token, OAuth, host allowlist, HTTP transport, output cap) are read by [`@ferrlabs/mcp-core`](https://github.com/FerrLabs/FerrLabs/tree/main/packages/mcp-core) and documented in the [FerrLabs MCP README](https://github.com/FerrLabs/FerrLabs/blob/main/packages/mcp/README.md#configuration).

## Development

```bash
cd mcp
pnpm install
pnpm build
pnpm test
pnpm smoke
```

`pnpm smoke` boots each server over stdio and checks the handshake and the advertised tools.

## License

Mozilla Public License 2.0, see [LICENSE](../LICENSE).
